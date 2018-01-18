# The MontiCore Language Worbench

## Links
* MontiCore website: http://www.monticore.de/, incl. 
  * various [MontiCore languages](http://www.monticore.de/languages/) and 
  * a [Getting Started](http://www.monticore.de/gettingstarted/) tutorial.
* MontiCore project: https://github.com/MontiCore/monticore

## Description

MontiCore is a full-fledged, open-source, language workbench for the design and realization of textual domain-specific languages (DSL). It enables the research of model-based software development methods employing a variety of DSLs and modeling languages. On top of this, MontiCore and its DSL products are successfully in use in academic and industrial research projects in various domains such as automotive software modeling, cloud architecture and security modeling, model-based robotics, smart energy management, neural network modeling. The design rationale of MontiCore is to provide a powerful and efficient workbench for the agile creation of DSLs along with their accompanying infrastructure such as analyses, transformations, and code generators. MontiCore features a functional and highly extensible architecture which allows to even further customize the DSL development process itself. 

## Example

At the core of each MontiCore language is a context-free grammar that defines abstract syntax and concrete syntax of the language under development in an integrated description. From this grammar, MontiCore generates model-processing infrastructure (e.g., parsers, abstract syntax classes) and additional infrastructure for well-formedness checking with Java rules as well as for template-based code generation using the [FreeMarker](https://freemarker.apache.org/) template engine.  

```
grammar Automaton extends de.monticore.lexicals.Lexicals {

  /** A ASTAutomaton represents a finite automaton
   * @attribute name Name of the automaton
   * @attribute states List of states
   * @attribute transitions List of transitions
   */
  symbol scope Automaton = "automaton" Name "{" (State | Transition)* "}" ;

  /** A ASTState represents a state of a finite automaton
   * @attribute name Name of state
   * @attribute initial True if state is initial state
   * @attribute final True if state is a final state
   * @attribute transitions List of transitions
   */
  symbol scope State = 
    "state" Name (("<<" ["initial"] ">>" ) | ("<<" ["final"] ">>" ))* ( ("{" Transition* "}") | ";") ;

  /** A ASTTransition represents a transition
   * @attribute from Name of the state from which the transitions starts
   * @attribute input Activation signal for this transition
   * @attribute to Name of the state to which the transitions goes
   */ 
  Transition =  from:Name "-" input:Name ">" to:Name ";" ;
}
```

From this grammar, MontiCore creates, among other infrastructure, the abstract syntax classes represented by the following [UML/P class diagram](http://www.se-rwth.de/topics/Unified-Modeling-Language.php):

```
classdiagram Automaton {

  public interface ASTAutomatonNode;

  public class ASTAutomaton {
    protected String name;
    protected java.util.List<Automaton.ASTState> states;
    protected java.util.List<Automaton.ASTTransition> transitions;
  }

  public class ASTState {
    protected String name;
    protected java.util.List<Automaton.ASTState> states;
    protected java.util.List<Automaton.ASTTransition> transitions;
    protected boolean initial;
    protected boolean r__final;
  }

  public class ASTTransition {
    protected String from;
    protected String input;
    protected String to;
  }

  enum AutomatonLiterals { FINAL, INITIAL; }
}
```

With the parsing infrastructure in place, models as the PingPong game automaton depicted below, can processed:

```
automaton PingPong {
  state NoGame <<initial>>;
  state Ping;
  state Pong <<final>>;

  NoGame - startGame > Ping;

  Ping - stopGame > NoGame;
  Pong - stopGame > NoGame;

  Ping - returnBall > Pong;
  Pong - returnBall > Ping;
}
```

Checking, for instance, that no automaton model yields two states of the same name, is impossible with context-free grammars. To achieve checking such properties nonetheless, MontiCore supports defining additional well-formedness rules in Java to reject malformed models after parsing.

```
package automaton.cocos;

import automaton._ast.ASTAutomaton;
import automaton._ast.ASTState;
import automaton._cocos.AutomatonASTAutomatonCoCo;
import de.se_rwth.commons.logging.Log;

public class UniqueStateNames implements AutomatonASTAutomatonCoCo {
  
  @Override
  public void check(ASTAutomaton automaton) {
    List<String> stateNames = new ArrayList<String>();
    
    for (ASTState state : automaton.getStates()) {
      if (!stateNames.contains(state.getName()) {
        stateNames.add(state.getName());
      }
      else {
        Log.error("0xA0124 The names of automaton states must be unique.", automaton.get_SourcePositionStart());
      }
    }
  }
  
}
```

Once a model has been checked, it can be processed further. This may include template-based code generation into executable GPL code, such as depicted below, where `${...}` and `<#>...</#>` delimit FreeMarker commands and `ast` contains the currently processed model.

```
import java.util.concurrent.ThreadLocalRandom;

class ${ast.getName()} {
  /**
   * Enum over states that supports retrieving states by name.
   */
  enum State {
    <#list ast.states as s> s("s")<#sep>, </#list>
    
    private String name;
    
    public State(String name) { this.name = name; }
    
    public static State fromName(String name) {  
      for (State s : State.values()) {
        if (s.name.equalsIgnoreCase(name)) { return s; }
      }
      return null;
    }
  }
  
  private State current = null;
  private List<State> initialStates = new ArrayList<State>(); 
  private List<State> finalStates = new ArrayList<State>();
  
  public ${ast.getName()}() {
    <#list ast.states as s>
      <#if s.initial> this.initialStates.add(State.${s.name}); </#if>
      <#if s.r__final> this.finalStates.add(State.${s.name}); </#if>
    </#list>
    this.current = this.initialStates(random(initialStates.size()));
  }
  
  private int random(int max) { return ThreadLocalRandom.current().nextInt(0, max); }
  
  private boolean isFinal(State s) { return this.finalStates.contains(s); }
  
  /**
   * Executes the automaton on the list of input words until a final state is reached
   * or no futher transition is possible.
   * @attribute inputs List of input words
   */
  public boolean exec(List<String> inputs) {
    // run until no further transition is possible
    for (int i = 0; i < inputs.size(); i++) {
      String currentInput = inputs.get(i);
      State nextState = null;
      <#list ast.states as s>
        <#list s.transitions as t>
          if (this.current.equals(State.${s.name}) && currentInput.equals(${t.input})) {
            System.out.println("Going from state ${s.name} to ${s.to} on word ${t.input}");
            nextState = State.${s.to});  
          } // end if
        </#list>
      </#list>
        if (this.nextState == null) { // not read until end, but no further transitions
          return false;
        }
        else {
          this.current = nextState;
        }
      } // end for
      
      // read the complete input, let's see whether reached state is final
      return this.isFinal(this.current));
    }
  }
```

## Key sources
* Pedram Mir Seyed Nazari:MontiCore: Efficient Development of Composed Modeling Language Essentials. In: Shaker Verlag, ISBN 978-3-8440-5320-3. Aachener Informatik-Berichte, Software Engineering, Band 29. 2017. [PDF](http://www.se-rwth.de/phdtheses/Diss-Nazari-MontiCore-Efficient-Development-of-Composed-Modeling-Language-Essentials.pdf)
* Arne Haber, Markus Look, Pedram Mir Seyed Nazari, Antonio Navarro Perez, Bernhard Rumpe, Steven Völkel, Andreas Wortmann:
Composition of Heterogeneous Modeling Languages. In: Model-Driven Engineering and Software Development, Communications in Computer and Information Science 580, pp. 45–66. Springer, 2015. [PDF](http://www.se-rwth.de/publications/Composition-of-Heterogeneous-Modeling-Languages.pdf)
* Holger Krahn, Bernhard Rumpe, Steven Völkel. MontiCore: a Framework for Compositional Development of Domain Specific Languages.
In: International Journal on Software Tools for Technology Transfer (STTT), Volume 12, Issue 5, pp. 353-372, September 2010. [PDF](http://www.se-rwth.de/publications/MontiCore-a-Framework-for-Compositional-Development-of-Domain-Specific-Languages.pdf)

## Ownership
* Open source project
* Mainly developed and maintained by the [Chair of Software Engineering](http://www.se-rwth.de/) at RWTH Aachen University
* Main contact [Prof. Dr. Bernhard Rumpe](http://www.se-rwth.de/)

