#Glossary of Software Language Engineering Terms


[Alphabetical Index | [Tag Index](Tag Index)] –  Important stuff is marked in **bold**, stuff with asterisk (\*) will not be on the exam.


##[Abstract data type](Abstract data type)
A type which is defined only through an interface of operations used to manipulate its value, and where the data representation is hidden.


[[Wikipedia](http://en.wikipedia.org/wiki/Abstract_data_type)]

##[Abstract syntax tree](Abstract syntax tree)
A tree representation of the syntactic structure of a sentence; similar to a [Parse tree](Parse tree), but usually ignoring literal [nonterminals](Nonterminal symbol) and nodes corresponding to [productions](Production rule) that don't directly contribute to the structure of the language (e.g., parenteses, productions used to encode [operator priority](Priority rule) and so on). May represent a slightly simpler language that the parse tree, for example, with operator calls [desugared](Desugaring) to function calls, and variations of a construct folded into one. Can be represented using as trees or terms, and described by an algebraic data type or a regular tree grammar. 


[[Wikipedia](http://en.wikipedia.org/wiki/Abstract_syntax_tree)]

##[Abstract value](Abstract value)
A value (of an [Abstract data type](Abstract data type)) known only through the operations used to create it. The user of an abstract data type operates on abstract values; only the implementation of the abstract data type sees the concrete value. A single abstract value may have many different concrete representations (either because there are several implementations of the data type, or because several representations mean the same –  for example, *1/2 = 2/4*)


##[Abstraction](Abstraction)
Focusing on relevant details while leaving irrelevant details unspecified or hidden away. *Data abstraction* (e.g., classes and interfaces) hides the details of how a data structure is represented, instead giving an interface to manipulate the data. *Control abstraction* (e.g., functions, methods) hides *how* something is done (the algorithm), focusing instead on *what* is done.


[[Wikipedia](http://en.wikipedia.org/wiki/Abstraction_(computer_science))]

##[Algebraic data type](Algebraic data type)
A [Composite data type](Composite data type) defined inductively from other types. Typically, each type has a number of cases or alternatives, which each case having a *constructor* with zero or more arguments. 
For example, ```data Expr = Int(int i) | Plus(Expr a, Expr b)```. The data type can be seen as an algebraic *signature*, with the expressions written using the constructors being *terms*. For our purposes, values may be interpreted as trees, with the constructor name being node labels, arguments being children, and atomic values and nullary constructors being leaves.


[[Wikipedia](http://en.wikipedia.org/wiki/Algebraic_data_type)]

##[Ambiguous grammar](Ambiguous grammar)
A grammar for which there is a string which has more than one leftmost [Derivation](Derivation).
#### Parsing
Requires a [Generalised parser](Generalised parser), produces a [Parse forest](Parse forest).
 


[[Wikipedia](http://en.wikipedia.org/wiki/Ambiguous_grammar)]

##[Analytic grammar](Analytic grammar)
A grammar which corresponds directly to the structure and semantics of a parser. For example, parser combinators and [parsing expression grammars](Parsing expression grammar) (PEGs).


[[Wikipedia](http://en.wikipedia.org/wiki/Formal_grammar#Analytic_grammars)]

##[Anonymous function](Anonymous function)
A function occuring as a value, without being bound (directly) to a name. C.f. [Closure](Closure).


##[Application binary interface](Application binary interface)
Specifies how software modules or components interact with each other at the machine code level. Typically includes such things function calling conventions (whether arguments are passed on the stack or in registers, and so on), the binary layout of data structures and how system calls are done.


[[Wikipedia](http://en.wikipedia.org/wiki/Application_binary_interface)]

##[Application programming interface](Application programming interface)
Specifies how software modules or components interact with each other.


[[Wikipedia](http://en.wikipedia.org/wiki/Application_programming_interface)]

##[Applicative programming](Applicative programming)



[[Wikipedia](http://en.wikipedia.org/wiki/Applicative_programming_language)]

##[Aspect-oriented programming](Aspect-oriented programming)



[[Wikipedia](http://en.wikipedia.org/wiki/Aspect-oriented_programming)]

##[Associativity](Associativity)
A property of binary operators in parsing, indicating whether expressions such as *a + b + c* should be interpreted as *(a + b) + b* (left associative), *a + (b + c)* (right associative) or as illegal (non-associative). 
#### Left
Operations are grouped on the left, giving a tree which is “heavy” on the left side; typically used for arithmetic operators. Without [associativity rules](Associativity rule), grammar productions usually look like ```PlusExpr ::= PlusExpr "+" MultExpr | ...```, forcing any expression containing the operator to be on the left side.
 
#### Right
Operations are grouped on the right, giving a tree which is “heavy” on the right side; typically used for assignment and exponentiation operators.



[[Wikipedia](http://en.wikipedia.org/wiki/Operator_associativity)]

##[Associativity rule](Associativity rule)
A [Disambiguation rule](Disambiguation rule) stating that an operator is either left-, right- or non-associative. E.g., in Rascal: ```syntax Expr = left (Expr "*" Expr | Expr "/" Expr );```


##[Attribute grammar](Attribute grammar)
A grammar where each production rule has attached attributes that are evaluated whenever the production rule is used in parsing. Can be used, for example, to build an intermediate representation directly from the parser, or to do typechecking while parsing.


[[Wikipedia](http://en.wikipedia.org/wiki/Attribute_grammar)]

##[Attribute-oriented programming](Attribute-oriented programming)



[[Wikipedia]([wiki)]

##[Backend](Backend)
The final stage of a compiler or language processor, often tasked with low-level optimisation and code generation targeted at a particular machine architecture.


##[Backus-Naur form](Backus-Naur form)
A formal notation for grammars, where productions are written ```<symbol> ::= <symbol1> "literal" ...```. Often extended with support for repetition (```*```, ```+```), optionality (```?``` or ```[]```) and alternatives (```|```).


[[Wikipedia](http://en.wikipedia.org/wiki/Backus-Naur_Form)]

##[Backward compatibility](Backward compatibility)



[[Wikipedia](http://en.wikipedia.org/wiki/Backward_compatibility)]

##[Bottom-up parser](Bottom-up parser)
A parser that works by identifying the lowest-level details first, rather than working [top-down](Top-down parser) from the start symbol. For example, an [LR parser](LR parser).


[[Wikipedia](http://en.wikipedia.org/wiki/Bottom-up_parser)]

##[Chomsky normal form](Chomsky normal form)
A simplified form of grammars where all the [production rules](Production rule) are of the form *A → BC* or *A → a* or *S → ε*, where *A*, *B* and *C* are [nonterminals](Nonterminal symbol) (with neither *B* nor *C* being the start symbol), *a* is a [terminal](Terminal symbol), *ε* is the empty string, and *S* is the [Start symbol](Start symbol). The third rule is only applicable if the empty string is in the language. Any [Context-free grammar](Context-free grammar) can be converted to this form, and any grammar in this form is context free.


[[Wikipedia](http://en.wikipedia.org/wiki/Chomsky_normal_form)]

##[Closure](Closure)
A function (or other operation) packaged together with all the variables it can access from the surrounding scope in which it was defined. A related term is [Anonymous function](Anonymous function), which does not necessarily imply access to variables from the surrounding scope.


[[Wikipedia](http://en.wikipedia.org/wiki/Closure_(computer_science))]

##[Code reuse](Code reuse)



[[Wikipedia](http://en.wikipedia.org/wiki/Code_reuse)]

##[Collection](Collection)



[[Wikipedia](http://en.wikipedia.org/wiki/Collection_(computing))]

##[Composite data type](Composite data type)
A data type constructed from other types (or itself, in the case of a [Recursive data type](Recursive data type)), e.g., a [structure](Structure, Structure type) or an [Algebraic data type](Algebraic data type).


##[Component-based software engineering](Component-based software engineering)
[


##[Container](Container)



[[Wikipedia](http://en.wikipedia.org/wiki/Container_(data_structure))]

##[Context-free grammar](Context-free grammar)
A formal grammar in which every [Production rule](Production rule) has a form of *A → w*, where *A* is a single [Nonterminal symbol](Nonterminal symbol) and *w* is a sequence of [terminals](Terminal symbol) and nonterminals.


[[Wikipedia](http://en.wikipedia.org/wiki/Context-free_grammar)]

##[Continuation](Continuation)
An abstract representation of the control state of a program. Often used in the sense of first-class continuations which let the execution state of a program be saved, passed around and then later restored.


[[Wikipedia](http://en.wikipedia.org/wiki/Continuation)]

##[Cross-cutting concern](Cross-cutting concern)
A programming concern, such as logging or security, which impacts many parts of a program and is difficult to decompose (cleanly separate into a library) from the rest of the system.


[[Wikipedia](http://en.wikipedia.org/wiki/Cross-cutting_concern)]

##[Dangling else problem](Dangling else problem)
A common ambiguity in programming languages (particularly those with C-like syntax) in which an optional ```else``` clause may be interpreted as belonging to more than one ```if``` sentence. Usually resolved in favour of the closest ```if```, often by an [Implicit disambiguation rule](Implicit disambiguation rule) (at least in non-[generalised parsing](Generalised parser)).


[[Wikipedia](http://en.wikipedia.org/wiki/Dangling_else)]

##[Declarative programming](Declarative programming)
A programming paradigm where programs are built by expressing the logic of a computation rather than the control flow; i.e., *what* should be done, rather than *how*.


[[Wikipedia](http://en.wikipedia.org/wiki/Declarative_programming)]

##[Definite clause grammar](Definite clause grammar)
A way of expressing grammars in logic programming languages such as Prolog.


[[Wikipedia](http://en.wikipedia.org/wiki/Definite_clause_grammar)]

##[Derivation](Derivation)
A sequence of [Production rule](Production rule) applications that rewrites the [Start symbol](Start symbol) into the input string (i.e., by replacing a [Nonterminal symbol](Nonterminal symbol) by its expansion at each step). This can be seen as a trace of a parser's actions or as a proof that the string belongs to the language. 
#### Leftmost
A derivation where the leftmost [Nonterminal symbol](Nonterminal symbol) is selected at every rewrite step.
 
#### Rightmost
A derivation where the rightmost [Nonterminal symbol](Nonterminal symbol) is selected at every rewrite step.
 


##[Desugaring](Desugaring)
Removal of [Syntactic sugar](Syntactic sugar). Sometimes used in a [Frontend](Frontend) to translate convenient language constructs used by the programmer into more fundamental constructs. For example, translating Java's enhanced ```for``` into a more basic iterator use.


[[Wikipedia](http://en.wikipedia.org/wiki/Syntactic_sugar)]

##[Deterministic context-free grammar](Deterministic context-free grammar)
A context-free grammar that can be derived from a deterministic pushdown automaton (DPDA). Always unambiguous.


[[Wikipedia](http://en.wikipedia.org/wiki/Deterministic_context-free_grammar)]

##[Diamond inheritance](Diamond inheritance)



[[Wikipedia](http://en.wikipedia.org/wiki/Multiple_inheritance#The_diamond_problem)]

##[Disambiguation rule](Disambiguation rule)
Used to resolve ambiguities in a grammar, so that the parser yields a single unambiguous parse tree. Includes techniques such as [follow restrictions](Follow restriction), [precede restrictions](Precede restriction), [priority rules](Priority rule), [associativity rules](Associativity rule), [keyword reservation](Reserve rule) and [implicit rules](Implicit disambiguation rule).


##[Domain-specific language](Domain-specific language)
A language (i.e., not just a library) with abstractions targeted at a specific problem domain. 
#### Benefits
Easier programming, more efficient or secure, possibly better error reports
 
#### Drawbacks
Lots of implementation work, language fragmentation, learning/training issues, less tooling, troublesome interoperability, possibly worse error reports
 
#### External DSL
A DSL defined as a separate programming language.
 
#### Internal or embedded DSL
A DSL defined as language-like interface to library.
 


[[Wikipedia](http://en.wikipedia.org/wiki/Domain-specific_language)]

##[Duck typing](Duck typing)
A typing style where the exact type of an object is not important, rather, any object is usable in any situation as long as it supports whatever methods are called on it. Used in many dynamic languages, such as Python, and in C++ templates. C.f. [structural typing](Structural type equivalence). 
Named after the *duck test* (attributed to James Whitcomb Riley): “When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck.”


[[Wikipedia](http://en.wikipedia.org/wiki/Duck_typing)]

##[Dynamic dispatch](Dynamic dispatch)
The process of selecting, at runtime, which implementation of a method to call at runtime; typically based on the the actual class of the object on which the method is called (as opposed to the static type of the variable). With *multiple dispatch*, the selection is done based on some or all arguments, making it a kind of runtime [Overload resolution](Overload resolution).


[[Wikipedia](http://en.wikipedia.org/wiki/Dynamic_dispatch)]

##[Dynamic language](Dynamic language)
A language where most or all of the language semantics is processed at runtime, including aspects such as [Name binding](Name binding) and [typing](Dynamic typing). May have features such as [Duck typing](Duck typing), [Dynamic typing](Dynamic typing), runtime reflection and introspection, and often allows code to be replaced and objects to be extended at runtime.


[[Wikipedia]()]

##[Dynamic scoping](Dynamic scoping)
When [names](Name binding) are resolved by finding the closest binding in the runtime environment (i.e., the execution stack), rather than in the local lexical environment (i.e., the containing scopes at the use site). C.f. [Lexical scoping](Lexical scoping).


[[Wikipedia](http://en.wikipedia.org/wiki/Scope_(computer_science)#Dynamic_scoping)]

##[Dynamic semantics](Dynamic semantics)
Gives the meaning of a program at execution time; either in terms of values being computed, actions being performed and so on.


[[Wikipedia](http://en.wikipedia.org/wiki/Programming_language#Dynamic_semantics)]

##[Dynamic typing](Dynamic typing)
When type safety is enforced at runtime. Values are associated with type information, which can also be used for other purposes, such as runtime reflection. Used in languages such as Python, Ruby, Lisp, Perl, etc. 
#### Benefits
Compiler may run faster; easy to load code dynamically at runtime; allows some things that are type safe but are still excluded by a static type system; easy to use [Duck typing](Duck typing) to get naturally generic code with little overhead for the programmer; reflection, introspection and metaprogramming becomes easier.
 
#### Drawbacks
Type errors cannot be detected at compile time; rigorous testing is needed to avoid type errors; some optimisations may be difficult to perform (less of a problem with [Just-in-time compilation](Just-in-time compilation)).
 Dynamic typing does not imply [Weak typing](Weak typing). C.f. [Static typing](Static typing), [Duck typing](Duck typing).


[[Wikipedia](http://en.wikipedia.org/wiki/Dynamic_typing)]

##[Encapsulation](Encapsulation)



[[Wikipedia](http://en.wikipedia.org/wiki/Encapsulation_(object-oriented_programming))]

##[Environment](Environment)
A mapping of names to values or types. Used in [evaluation](Evaluator) and [typechecking](Typechecker) to carry name bindings. The environment is passed around (propagated) according to the scoping rules of the language, and may use a more complicated data structure to accomodate nested and/or named [scopes](Scope).


[[Wikipedia](http://en.wikipedia.org/wiki/Environment_(type_theory))]

##[Epsilon](Epsilon)
In a grammar, the empty string.


##[Evaluator](Evaluator)
A program that executes another program.


[[Wikipedia](http://en.wikipedia.org/wiki/Interpreter_(computing))]

##[Event-driven programming](Event-driven programming)



[[Wikipedia](http://en.wikipedia.org/wiki/Event-driven_programming)]

##[Extended Backus-Naur form](Extended Backus-Naur form)
A syntax notation introduced by Niklaus Wirth, which includes notation for optionality, repetition, alternation, grouping and so on.


[[Wikipedia](http://en.wikipedia.org/wiki/Extended_Backus-Naur_Form)]

##[Field](Field)
A data [Member](Member) of a data structure.


##[Follow restriction](Follow restriction)
A disambiguation technique where a symbol is forbidden from or forced to be immediately followed by a certain terminal.


##[Formation rule](Formation rule)
A [Grammar](Grammar); rules for describing which strings are valid in a language. This term is used mainly in logic.


##[Frontend](Frontend)
The first stage of a compiler or language processor, typically including a [Parser](Parser) (possibly with a [Tokeniser](Tokeniser)), and a [Typechecker](Typechecker) (semantic analyser). Sometimes also includes [Desugaring](Desugaring). Is typically responsible for giving the programmer feedback on errors, and translating to the internal [AST](Abstract syntax tree) or representation used by the rest of the system.


##[Function](Function)
An abstraction over expressions (or more generally, over expressions, statements and algorithms).


##[Function type](Function type)
The representation of a function in the type system. Typically includes parameter types and return type, written *t_1, ..., t_k → t*.


##[Function value](Function value)
The representation of a function in an evaluator or in a dynamic semantics specification. Usually includes the parameter names and the function body. Forms a [Closure](Closure) together with an environment giving the function's declaration scope.


##[Functional programming](Functional programming)
A programming paradigm based on mathematical functions, usually without state and mutable variables. Pure functional languages have [Referential transparency](Referential transparency), and typically allows [Higher-order functions](Higher-order function).


[[Wikipedia](http://en.wikipedia.org/wiki/Functional_programming)]

##[Generalized multitext grammar](Generalized multitext grammar)
A synchronous grammar formalism that is weakly equivalent to [Linear Context-Free Rewriting Systems](Linear Context-Free Rewriting Systems) (LCFRS), but retains much of the notational and intuitive simplicity of a [Context-Free Grammar](Context-Free Grammar) (CFG).


[Paper: [Generalized multitext grammars](http://dx.doi.org/10.3115/1218955.1219039)]

##[Generalised parser](Generalised parser)
A parser that can handle the full range of [context-free grammars](Context-free grammar), including nondeterministic and ambiguous grammars. For example, a [GLL parser](GLL parser) or a [GLR parser](GLR parser).


##[Generative grammar](Generative grammar)
An approach to specifying the syntax of the language, using a set of rules that produce all strings in the language. Formalised by Noam Chomsky in the late 1950s, but the idea goes back to Pāṇini's Sanskrit grammar, 4th century BCE.


[[Wikipedia](http://en.wikipedia.org/wiki/Generative_grammar)]

##[Generative programming](Generative programming)
Programming aided by automatic generation of code, including techniques such as [Generic programming](Generic programming), templates, aspects, code generation, etc.


[[Wikipedia](http://en.wikipedia.org/wiki/Automatic_programming#Generative_programming)]

##[Generic programming](Generic programming)
A programming style which allows the same piece of code to deal with many different types, for example through [Polymorphism](Polymorphism).


[[Wikipedia](http://en.wikipedia.org/wiki/Generic_programming)]

##[GLL parser](GLL parser)
An [LL parsing](LL parser) algorithm extended to handle nondeterministic and ambiguous grammars, making it capable of parsing any context-free grammar. Unlike normal LL or [recursive descent parsers](Recursive descent parser), it can also handle [Left recursion](Left recursion).


[[WWW](http://www.rhul.ac.uk/computerscience/research/csle/gllparsers.aspx)]

##[GLR parser](GLR parser)
An [LR parsing](LR parser) algorithm extended to handle nondeterministic and ambiguous grammars, making it capable of parsing any context-free grammar.


[[Wikipedia](http://en.wikipedia.org/wiki/Glr_parser)]

##[Grammar](Grammar)
A formal set of rules defining the syntax of a language. Formally, a tuple *〈N,T,P,S〉* of [nonterminal symbols](Nonterminal symbol) *N*, [terminal symbols](Terminal symbol) *T*, [production rules](Production rule) *P*, and a [Start symbol](Start symbol) *S ∈ N*. In software languages, the most frequently used kinds are [context-free](Context-free grammar) and [regular grammars](Regular expression).


[[Wikipedia](http://en.wikipedia.org/wiki/Formal_grammar)]

##[Grammar in a broad sense](Grammar in a broad sense)
A structural description in software systems, and a description of structures used in software systems: a parser specification is an enriched grammar, a type definition is a grammar, an attribute grammar comprises a grammar, a class diagram can be considered a grammar, a metamodel must contain a grammar, an algebraic signature is a grammar, an algebraic data type is a grammar, a generalized algebraic data type is a very powerful grammar, a graph grammar and a tree grammar are grammars for visual concrete notation, an object grammar contains two grammars and a mapping between them.


[Paper: [Toward an Engineering Discipline for Grammarware](http://doi.acm.org/10.1145/1073000)]

##[Greibach normal form](Greibach normal form)



[[Wikipedia](http://en.wikipedia.org/wiki/Greibach_normal_form)]

##[Higher-order function](Higher-order function)
A function which takes takes functions as arguments or returns [function values](Function value).


[[Wikipedia](http://en.wikipedia.org/wiki/Higher-order_function)]

##[Imperative programming](Imperative programming)
A programming paradigm based on statements that change program state; as opposed to [Declarative programming](Declarative programming). May be combined with [Object-oriented programming](Object-oriented programming).


[[Wikipedia](http://en.wikipedia.org/wiki/Imperative_programming)]

##[Implicit disambiguation rule](Implicit disambiguation rule)
A [Disambiguation rule](Disambiguation rule) built into the parser, such as longest match for regular expressions, or resolving the [Dangling else problem](Dangling else problem) by preferring shift over reduce in an [LR parser](LR parser).


##[Inheritance](Inheritance)
A technique in [Object-oriented programming](Object-oriented programming) which combines automatic code reuse with subtyping.


[[Wikipedia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))]

##[Inlining](Inlining)
A technique in language processing where a call to a function or procedure is replaced by the code being called. Often used as part of code [Optimisation](Optimisation); removes [Abstraction](Abstraction) introduced by the programmer.


[[Wikipedia](http://en.wikipedia.org/wiki/Inlining)]

##[Intentional programming](Intentional programming)



[[Wikipedia](http://en.wikipedia.org/wiki/Intentional_programming)]

##[Island grammar](Island grammar)
A grammar which describes only small parts of a language, skipping over the rest. Used, for example, to recover documentation from a program.


[[Program Transformation Wiki](http://www.program-transformation.org/Transform/IslandGrammars)]

##[Just-in-time compilation](Just-in-time compilation)
A technique used in interpreted or bytecode-compiled languages where program code is compiled at runtime, during evaluation. This gives the speed advantages of compilation, while retaining the dynamic flexibility and architecture-independence of a interpreted or bytecode-based language. Can sometimes give even better performance than static compilation, since more information may be available at runtime, leading to better optimisation. Heavily used in modern implementations of JVM and .NET.


[[Wikipedia](http://en.wikipedia.org/wiki/Just-in-time_compilation)]

##[Kleene closure](Kleene closure)
A metasyntactic sugar for repetition: ```x*``` means that ```x``` can be repeated zero or more times. The language that the Kleene star generates, is a monoid with concatenation as the binary operation and epsilon as the identity element.


[[Wikipedia](http://en.wikipedia.org/wiki/Kleene_star)]

##[Language](Language)
A system of communication, with structure ([syntax](Tag Index#markdown-header-Syntax)) and meaning ([semantics](Tag Index#markdown-header-Semantics)), and [abstractions](Abstraction) that allow you to communicate usefully at different levels (i.e., more than just pointing at concrete things or showing a picture of something –  where the meaning would be the thing itself).


##[Left associativity](Left associativity)



[[Wikipedia](http://en.wikipedia.org/wiki/Operator_associativity)]

##[Left factoring](Left factoring)
A technique used to avoid backtracking in [top-down parsing](Top-down parser), by ensuring that the productions for a [nonterminal](Nonterminal symbol) don't have alternatives that start with the same [terminals](Terminal symbol).. Used to [massage](Massaging) a grammar to [LL](LL parser) form.


[[Wikipedia](http://en.wikipedia.org/wiki/Left_factoring)]

##[Left recursion](Left recursion)
When production rules have the form *A → Aa | b*, with the [Nonterminal](Nonterminal symbol) occurring directly or indirectly to the left of all [terminals](Terminal symbol) on the right-hand side. Must be eliminated in order to use an [LL parser](LL parser).


[[Wikipedia](http://en.wikipedia.org/wiki/Left_recursion)]

##[Lexeme](Lexeme)
A string of characters that is significant as a group; a word or [Token](Token).


##[Lexical analysis](Lexical analysis)
Converting a sequence of characters (letters) to a sequence of [tokens](Token) ([lexemes](Lexeme) or words).


##[Lexical scoping](Lexical scoping)
When [names](Name binding) are resolved (possibly statically) by finding the closest binding in the lexical environment (i.e., by looking at the scopes that lexically contains the name). C.f. [Dynamic scoping](Dynamic scoping).


[[Wikipedia](http://en.wikipedia.org/wiki/Scope_(computer_science)#Lexical_scoping)]

##[Lexical syntax](Lexical syntax)
Describes (often using a [Regular grammar](Regular expression)) the syntax of [tokens](Token); e.g., what constitutes an identifier, a number, different operators and the whitespace that separates them.


[[Wikipedia](http://en.wikipedia.org/wiki/Lexical_grammar)]

##[Literate programming](Literate programming)
A programming style where programs can be read as documents that explain the implementation, with explanations in a natural language. Tools allow programs to be compiled as either code or documents.


[[Wikipedia](http://en.wikipedia.org/wiki/Literate_programming)]

##[LL parser](LL parser)
A table-driven [Top-down parser](Top-down parser), similar to a [Recursive descent parser](Recursive descent parser). Has trouble dealing with [Left recursion](Left recursion) in production rules, so the grammar must typically be [left factored](Left factoring) prior to use. The LL parser reads its input in one direction (left-to-right) and produces a leftmost [Derivation](Derivation), hence the name LL. Often referred to as LL(*k*), where the *k* indicates the number of tokens of lookahead the parser uses to avoid backtracking.


[[Wikipedia](http://en.wikipedia.org/wiki/LL_parser)]

##[LL grammar](LL grammar)
A grammar that can be parsed by a [LL parser](LL parser).


[[Wikipedia](http://en.wikipedia.org/wiki/LL_grammar)]

##[Logic programming](Logic programming)
A [Declarative programming](Declarative programming) paradigm based on formal logic, inference and reasoning. Useful for many purposes, including formal specification of language semantics. Prolog is the most well-known logic language.


[[Wikipedia](http://en.wikipedia.org/wiki/Logic_programming)]

##[LR parser](LR parser)
A [Bottom-up parser](Bottom-up parser) that can handle deterministic context-free languages in linear time. Common variantes are LALR parsers and SLR parsers. It reads its input in one direction (left-to-right) and produces a rightmost [Derivation](Derivation), hence the name LR.


[[Wikipedia](http://en.wikipedia.org/wiki/LR_parser)]

##[LR grammar](LR grammar)
A grammar that can be parsed by a [LR parser](LR parser).


[[Wikipedia](http://en.wikipedia.org/wiki/LR_grammar)]

##[Massaging](Massaging)
The act of modifying a grammar to make it fit a particular technology or purpose.


##[Megamodel](Megamodel)
A result of megamodelling —  a model which elements are software languages, models, metamodels, transformations, etc


[Paper: [On the Need for Megamodels](http://www.softmetaware.com/oopsla2004/bezivin-megamodel.pdf)]

##[Member](Member)
An element of a structure or class; a [Field](Field), [Method](Method) or inner class/type.


##[Method](Method)
A function which is a [Member](Member) of a class. Typically receives a self-reference to an object as an implicit argument.


[[Wikipedia](http://en.wikipedia.org/wiki/Method_(computer_programming))]

##[Mixin](Mixin)
A partial class (data fields and methods) that can be used to plug functionality into another class using inheritance.


[[Wikipedia](http://en.wikipedia.org/wiki/Mixin)]

##[Multi-paradigm programming](Multi-paradigm programming)
Programming which combines several paradigms, such as functional, imperative, object-oriented or logic programming. Languages that support multiple paradigms include, for example, C++, Scala, Oz, Lisp, and many others.


[[Wikipedia](http://en.wikipedia.org/wiki/Programming_paradigm#Multi-paradigm_programming_language)]

##[Name binding](Name binding)
A part of language processing where names are associated with their declarations, according to [scoping](Scope) and [Namespace](Namespace) rules. A name's binding is typically determined by checking the [Environment](Environment) at the use site. 
#### Static
When done statically (or *early*), name binding is often combined with [typechecking](Typechecker).
 
#### Dynamic
Names are bound at runtime; also applies to [Dynamic dispatch](Dynamic dispatch) (where it is sometimes called *late* or *virtual* binding), where certain properties (such as types) may be known statically, but the exact operation called is determined at runtime.



[[Wikipedia](http://en.wikipedia.org/wiki/Name_binding)]

##[Named tuple](Named tuple)
A tuple where the elements are named, like in a [structure](Structure, Structure type). Often exhibits [Structural type equivalence](Structural type equivalence), even in languages that normally use [Nominative type equivalence](Nominative type equivalence)


##[Namespace](Namespace)
Some kind name grouping that makes it possible to distinguish different uses of the same name. For example, having variable names be distinct from type names; or treating names in one module as distinct from the same names in another module (In this sense, namespaces are related to [Scope](Scope)).


[[Wikipedia](http://en.wikipedia.org/wiki/Namespace)]

##[Nominative type equivalence](Nominative type equivalence)
A system where type equivalence or compatibility is determined based on the type names (or, more strictly, which declaration the names refer to) and not the structure of the type. C.f. [Structural type equivalence](Structural type equivalence).


[[Wikipedia](http://en.wikipedia.org/wiki/Nominative_type_system)]

##[Nonterminal footprint](Nonterminal footprint)
A non-recursive measure of [Nonterminal symbol](Nonterminal symbol) usage in a grammatical expression: a multiset of presence indicators (```1``` for the nonterminal itself, ```?``` for its optional use, ```*``` for its [Kleene closure](Kleene closure), etc). A usefulness of a footprint for grammar matching depends on how rich the metalanguage is.


##[Nonterminal symbol](Nonterminal symbol)
A symbol in a grammar which is defined by a production. Can be replaced by terminal symbols by applying the production rules of the grammar. In a [Context-free grammar](Context-free grammar), the left-hand side of a production rule consists of a single nonterminal symbol.


##[Object-oriented programming](Object-oriented programming)
A programming paradigm based on modelling interactions between objects. Objects have [fields](Field) and [methods](Method) and encapsulate state. Provides [data abstraction](Abstract data type), and usually supports [Inheritance](Inheritance), [Dynamic dispatch](Dynamic dispatch) and subtype [subtype polymorphism](Polymorphism).


[[Wikipedia](http://en.wikipedia.org/wiki/Object-oriented_programming)]

##[Optimisation](Optimisation)
The process of transforming program code to make it more efficient, in terms of time or space or both.


[[Wikipedia](http://en.wikipedia.org/wiki/Program_optimization)]

##[Overloading](Overloading)
When the same name is used for multiple things (of the same kind). For example, several functions with the same name, distinguished based on the parameter types. C.f. [Namespace](Namespace), where the same name can have different meanings in different context (e.g., type names are distinct from variable names).


[[Wikipedia](http://en.wikipedia.org/wiki/Function_overloading)]

##[Overload resolution](Overload resolution)
A compilation step, usually combined with typechecking, where the name of an overloaded function is resolved based on the types of the actual arguments. C.f. [Dynamic dispatch](Dynamic dispatch), which does something similar at runtime.


[[Wikipedia](http://en.wikipedia.org/wiki/Function_overloading)]

##[Parse forest](Parse forest)
The [parse trees](Parse tree) that are the result of parsing an ambiguous grammar using a [Generalised parser](Generalised parser).


##[Parse tree](Parse tree)
A tree that shows the structure of a string according to a grammar. The tree contains both the tokens of the original string, and a trace of the derivation steps of the parse, thus showing how the string is a valid parse according to the grammar. Typically, each leaf corresponds to a [Token](Token), each interior node corresponds to a [Production rule](Production rule), and the root node to a production rule of the [Start symbol](Start symbol).


##[Parser](Parser)
A program that recognises input according to some [Grammar](Grammar), checking that it conforms to the syntax and builds a structured representation of the input.


##[Parser combinator](Parser combinator)
A way of expression a grammar and a parser using [higher-order functions](Higher-order function). Each combinator accepts parsers as arguments and returns a parser.


[[Wikipedia](http://en.wikipedia.org/wiki/Parser_combinator)]

##[Parsing expression grammar](Parsing expression grammar)
A form of [Analytic grammar](Analytic grammar), giving rules that can be directly applied to parse a string [top-down](Top-down parser). Similar to a [Context-free grammar](Context-free grammar), but the rules are unambiguously interpreted; for example, alternatives are tried in order. Related to [parse combinators](Parser combinator). PEGs are a useful and straight-forward technique for parsing software languages.
It is suspected that there are context-free languages that cannot be parsed by a PEG, but this has not been proved.


[[Wikipedia](http://en.wikipedia.org/wiki/Parsing_expression_grammar)]

##[Parsing](Parsing)
Recovering the grammatical structure of a string. The task done by a [Parser](Parser).


##[Pattern matching](Pattern matching)
A technique for comparing (typically [algebraic](Algebraic data type)) data structures, where one or both structures may contain variables (sometimes refered to as meta-variables). Upon successful match, variables are bound to the corresponding substructure from the other side. Related to [Unification](Unification) in Prolog, but often more restricted.


[[Wikipedia](http://en.wikipedia.org/wiki/Pattern_matching)]

##[Polymorphism](Polymorphism)
 
#### Ad hoc
Another name for function or operator [Overloading](Overloading).
 
#### Parametric
When a function or data type is [generic](Generic programming) and handle values of different types in the same; for instance, ```List<T>``` –  a list with elements of an arbitrary type. The specific type in question is often given as a parameter, hence the name.
 
#### Subtype
When an object belonging to a subtype can be used in a place where the supertype is expected (as with classes and inheritance in [Object-oriented programming](Object-oriented programming)).
 


[[Wikipedia](http://en.wikipedia.org/wiki/Polymorphism_(computer_science))]

##[Precede restriction](Precede restriction)
A disambiguation technique where a symbol is forbidden from or forced to be immediately preceded by a certain terminal.


##[Predictive parser](Predictive parser)
A [Recursive descent parser](Recursive descent parser) which does not require backtracking. Instead, it looks ahead a finite number of tokens and decides which parsing function should be called next. The grammar must be LL(*k*) for this to work, where *k* is the maximum lookahead.


##[Prime normal form](Prime normal form)



[Paper: [Prime Normal Form and Equivalence of Simple Grammars](http://dx.doi.org/10.1007/11605157_7)]

##[Priority rule](Priority rule)
A [Disambiguation rule](Disambiguation rule) declaring an operator's priority/precedence. E.g., in Rascal: ```syntax Expr = Expr "*" Expr > Expr "+" Expr;```


##[Procedural programming](Procedural programming)
A programming paradigm based around procedure calls. Sometimes considered the same as [Imperative programming](Imperative programming) and typically based on [Structured programming](Structured programming).


[[Wikipedia](http://en.wikipedia.org/wiki/Procedural_programming)]

##[Production rule](Production rule)
A rule describing which symbols may replace other symbols in a grammar. In a [Context-free grammar](Context-free grammar), the left-hand side consists of a single [Nonterminal symbol](Nonterminal symbol), while the right-hand side may be any sequence of [terminals](Terminal symbol) and nonterminals. For example, ```Expr ::= Expr "+" Expr``` says that anywhere you may have an expression, you can have an expression plus another expression. The rules may be used to generate syntactically correct strings, by applying them as rewrite rules starting with the [Start symbol](Start symbol), or be used to parse strings, e.g., in a [Top-down parser](Top-down parser) or [Bottom-up parser](Bottom-up parser).


##[Program slicing](Program slicing)
A program transformation technique where all code that is irrelevant to a certain set of inputs or outputs is removed. Applied *forwards*, any code not directly or indirectly using a selection of inputs is discarded; applied *backwards*, all code that does not contribute to the computation of the selected outputs is discarded. Mainly used in debugging (e.g., to find the code that might have contributed to an error), but sometimes also as an optimisation technique. Originally formalised by Mark Weiser in the early 1980s.


[[Wikipedia](http://en.wikipedia.org/wiki/Program_slicing)]

##[Right associativity](Right associativity)



[[Wikipedia](http://en.wikipedia.org/wiki/Operator_associativity)]

##[Recogniser](Recogniser)
A program that recognises input according to some [Grammar](Grammar), giving an error if it does not conform to the grammar, but does not build a data structure.


##[Record, Record type](Record, Record type)
See [Structure, Structure type](Structure, Structure type).


[[Wikipedia](http://en.wikipedia.org/wiki/Record_(computer_science))]

##[Recursive data type](Recursive data type)
A [Composite data type](Composite data type), such as an [Algebraic data type](Algebraic data type) which may contain itself. Used, for example, to define data structures such as lists and trees.


##[Recursive descent parser](Recursive descent parser)
A [Top-down parser](Top-down parser) built from mutually recursive functions, where each function typically implements one [Production rule](Production rule) of the grammar.


[[Wikipedia](http://en.wikipedia.org/wiki/Recursive_descent_parser)]

##[Referential transparency](Referential transparency)
When an expression can be replaced by its value without changing the meaning of the program; i.e., it will evaluate to the same value every time and not cause side effects. Usually a property of [Functional programming](Functional programming) languages.


[[Wikipedia](http://en.wikipedia.org/wiki/Referential_transparency_(computer_science))]

##[Regular expression](Regular expression)
A formalism for describing a [Regular grammar](Regular grammar), using the normal alphabet mixed with special *metasyntactic* symbols, such as the [Kleene star](Kleene closure). Commonly used to specify the [Lexical syntax](Lexical syntax) of a language, and also for searching and string matching in many different applications. 


[[Wikipedia](http://en.wikipedia.org/wiki/Regular_expression)]

##[Regular grammar](Regular grammar)
A formal [Grammar](Grammar) where every production rule has the form *A → aB* (for a *right regular grammar*), or *A → a* or *A → ε*, where *A* and *B* are [nonterminal symbols](Nonterminal symbol) and *a* is a [Terminal symbol](Terminal symbol), and *ε* is the [empty string](Epsilon). Alternatively, the first production form may be *A → Ba*, for a *left regular grammar*.
#### Limitations
Can't express arbitrary nesting, such as nested parentheses or block structure in a language.



[[Wikipedia](http://en.wikipedia.org/wiki/Regular_grammar)]

##[Remote attribute grammar](Remote attribute grammar)



[Paper: [Remote attribute grammars](http://dx.doi.org/10.1145/1082036.1082042)]

##[Reserve rule](Reserve rule)
A [Disambiguation rule](Disambiguation rule) which states that a grammar symbol cannot match some constraint. For example, identifiers could be defined as any word matching ```[a-zA-Z]+``` *except* ```if```, ```while```, ...


##[Scannerful parsing](Scannerful parsing)
Parsing is divided into two parts; a [Tokeniser](Tokeniser) that deals with the lexical syntax and a parser that deals with the sentence syntax. 
#### Benefits
Faster than scannerless parsing, because the lexical syntax is specified with a regular grammar which can be parsed very efficiently.
 
#### Drawbacks
Cannot deal with arbitrary composition of languages.
 


##[Scannerless parsing](Scannerless parsing)
When scanning and parsing is unified into one process that deals with with the input characters directly. 
#### Benefits
Can parse combinations of languages that have different lexical syntax. Lexical syntax can be context-free, not just regular.
 
#### Drawbacks
Slower than scannerful parsing. Can lead to hard to find lexical ambiguities.
 


##[Scope](Scope)
 A collection of identifier bindings –  i.e., what is captured by the [Environment](Environment) at some point in the code or in time. 
#### Nested
With nested scopes, variables in inner scopes may *shadow* those in outer scopes, and variables are removed as control flows out of the scope. Variable shadowing may be forbidden in some languages.
 
#### Named
With named scopes, we can refer to names in scopes that are not ancestors of the current scope. For example, with C++ classes and namespaces and Java packages and (static) classes.
 


[[Wikipedia](http://en.wikipedia.org/wiki/Scope_(programming))]

##[Sentential form](Sentential form)



[[Wikipedia](http://www.seas.upenn.edu/\unhbox \voidb@x \penalty \@M &nbsp;{}cit596/notes/dave/cfg7.html)]

##[Semantic analysis](Semantic analysis)
A phase of language processing that enforces the [Static semantics](Static semantics) of a language. Includes [typechecking](Typechecker), [Name binding](Name binding), [Overload resolution](Overload resolution) and checking other static constraints.


##[Sequential coupling](Sequential coupling)



[[Wikipedia](http://en.wikipedia.org/wiki/Sequential_coupling)]

##[Software Language](Software Language)
An artificial [language](Language) used in software development.For example, Java (programming language), HTML (markup language), XML (data language), CSS (domain-specific language).


##[Start symbol](Start symbol)
The [Nonterminal symbol](Nonterminal symbol) in a grammar that generates all valid strings in a language.


##[Static semantics](Static semantics)
The part of language semantics which is processed at compile time (statically). Often includes constraints that might be part of the syntax, but which is done separately in order to keep the grammar [context-free](Context-free grammar). Includes concepts like [Name binding](Name binding) and [Typechecking](Typechecking), and is used to eliminate a large class of invalid or erroneous programs. See [Static typing](Static typing).


##[Static typing](Static typing)
When [Type safety](Type safety) is enforced at compile type (though some tests, such as for [Typecasting](Typecasting), may be done at runtime). 
#### Benefits
Detects a large an important class of errors (type errors) at compile time; enables advanced optimisations and efficient memory use.
 
#### Drawbacks
Type system may become either overly complicated or overly restrictive; doesn't help with non-type errors; makes dynamic loading of code somewhat more complicated; type declarations may be cumbersome if the language lacks [Type inference](Type inference).
 


##[Strong typing](Strong typing)
When a language (to some degree) enforces [Type safety](Type safety).


[[Wikipedia](http://en.wikipedia.org/wiki/Strong_typing)]

##[Structural type equivalence](Structural type equivalence)
A system where two types are equal or compatible if they have the same structure; e.g., have the same fields with the same types in the same order. C.f. [Nominative type equivalence](Nominative type equivalence).


[[Wikipedia](http://en.wikipedia.org/wiki/Structural_type_system)]

##[Structure, Structure type](Structure, Structure type)
A [Composite data type](Composite data type) with named [fields](Field) [members](Member); such as ```struct``` in C or ```record``` in Pascal. Similar to (or same as, with [Structural type equivalence](Structural type equivalence)) a [Named tuple](Named tuple).


[[Wikipedia](http://en.wikipedia.org/wiki/Record_(computer_science))]

##[Structured programming](Structured programming)
A programming paradigm where the clarity of programs is improved by nestable language constructs like ```if```, ```while```, as opposed to conditional jumps.


[[Wikipedia](http://en.wikipedia.org/wiki/Structured_programming)]

##[Syntactic sugar](Syntactic sugar)
See [Desugaring](Desugaring).


[[Wikipedia](http://en.wikipedia.org/wiki/Syntactic_sugar)]

##[Technological space](Technological space)



[Paper: [Technological Spaces: an Initial Appraisal](http://eprints.eemcs.utwente.nl/10206/01/0363TechnologicalSpaces.pdf)]

##[Terminal symbol](Terminal symbol)
An elementary symbol in the language defined by a grammar, which cannot be changed/matched by the production rules in the grammar (i.e., the symbol doesn't occur (alone) on the left-hand side of a production). Corresponds to a [Token](Token) or an element of the alphabet of a language.


##[Token](Token)
A [Lexeme](Lexeme) or group of characters that forms a basic unit of parsing, categorised according to type, e.g., identifier, number, addition operator, etc. Forms the alphabet of the parser in [Scannerful parsing](Scannerful parsing).


[[Wikipedia](http://en.wikipedia.org/wiki/Token_(parser)#Token)]

##[Tokeniser](Tokeniser)
A program that performs [Lexical analysis](Lexical analysis), grouping and classifying input into [tokens](Token).


##[Top-down parser](Top-down parser)
A parser using a strategy where the top-level constructs are recognised first, starting with the start symbol. The parser starts at the root of the parse tree, and builds it top-down, according to the rules of the grammar. Includes [LL parsing](LL parser) and [Recursive descent parsing](Recursive descent parser).


##[Trait](Trait)



[[Wikipedia](http://en.wikipedia.org/wiki/Trait_(computer_programming))]

##[Traversal](Traversal)
Visiting the nodes/parts of a data structure such as a tree


##[Typechecker](Typechecker)
A program that detects type errors, ensuring [Type safety](Type safety) in a [statically typed](Static typing) language. Often combined with other static semantic checks and processing, such as [Overload resolution](Overload resolution), [Name binding](Name binding), and checking access restrictions on names. Can be seen as a form of abstract interpretation, where the abstract values are the types, and all operations are defined according to their static semantics (i.e., type signature) rather than their dynamic semantics. C.f. [Semantic analysis](Semantic analysis).


[[Wikipedia](http://en.wikipedia.org/wiki/Typechecker)]

##[Type inference](Type inference)
Automatic deduction of the types in a language. Used with [Static typing](Static typing) to avoid having to declare types for variables and functions. Particularly useful in [Generic programming](Generic programming) and type [Polymorphism](Polymorphism) where type expressions can become quite complicated. Used, e.g., in Haskell and Standard ML.


[[Wikipedia](http://en.wikipedia.org/wiki/Type_inference)]

##[Type safety](Type safety)
Whether a language protects against type errors, such as when a value of one data type is interpreted as another type (e.g., an ```int``` as a ```float``` or as a pointer to a string).


[[Wikipedia](http://en.wikipedia.org/wiki/Type_safety)]

##[Type system](Type system)
Defines how a language classifies expressions and values into types.


[[Wikipedia](http://en.wikipedia.org/wiki/Type_system)]

##[Typecasting](Typecasting)
Forced conversion from one type to another. In a languages with [weaker](Weak typing) type systems, may lead to data being misinterpreted.


[[Wikipedia](http://en.wikipedia.org/wiki/Type_conversion)]

##[Unification](Unification)
A [Pattern matching](Pattern matching)-like operation, where the goal is to find an assignment of variables so that two terms become equal. Used heavily in Prolog.


[[Wikipedia](http://en.wikipedia.org/wiki/Unification_(computing))]

##[Viable prefix](Viable prefix)



[[Wikipedia](http://en.wikipedia.org/wiki/Viable_prefix)]

##[Virtual inheritance](Virtual inheritance)



[[Wikipedia](http://en.wikipedia.org/wiki/Virtual_inheritance)]

##[Weak typing](Weak typing)
When a language (to some degree) does not enforces [Type safety](Type safety).


[[Wikipedia](http://en.wikipedia.org/wiki/Weak_typing)]

##[Yield](Yield)
The *yield* of a parsetree is the unparsed input text.


[Alphabetical Index | [Tag Index](Tag Index)]


