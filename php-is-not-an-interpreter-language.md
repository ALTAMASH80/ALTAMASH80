#PHP is not an interpreter language

PHP is not an interpreter language as people accuse it to be. People who like to call PHP an interpreter language don't even know what an interpreter language is. They claim PHP is both interpreter and compiled. Which in my assessment are all lies or a made-up thing. Now, to understand what is an interpreter and a compiled language we've to ask ourselves what makes a language compiled language and what makes an interpreter language.

##Question: What is a compiled language?

The answer you'll get is the language like C++, Java, CSharp etc. The thing to note here is they didn't specify why they're called complied language. The answer they give is that C++ produce executable file so does JAVA and CSharp. What?

Executables are dependent on the operating system. Executables are needed in Windows, what about Apple and Linux? So, this argument is not valid at all. So, what is a compiled and interpreter language and where can we find them and compare the two for a better assessment.

##Compiled Language: (According to PhD in the compiler)

Compiled languages are the languages that compile the code as a whole and then produce the result. Meaning they will first analyse the whole code and then execute it.

##Interpreter Language:

Interpreter language is a language that compiles and executes the code at the same time but a statement by statement or a statement in multiple lines.

The above is the basic definitions that determine a language as an interpreter or compiled. Now let us assess PHP with the above language definition.

```
<?php
echo 'First Line';

echo 'Second Line' . break the code
echo 'Third Line';
```

Now, if we analyse the above code via an interpreter approach. **What interpreter says that a statement gets executed immediately after it's complied. Therefore, we should see "The first line" as an output on the browser. But do we see it? The answer is no.** It gives a fatal error message. This is sufficient to prove that PHP is not an interpreter language. But even with this example, people will not be satisfied. So, I've to give them where they can find an interpreter in today's time.


The answer is simple MySQL. MySql compiler which executes SQL is an interpreter compiler. Furthermore, if you want to explore interpreter language ask someone to give you GW-BASIC and you'll see what I mean. As I don't even know GW-BASIC even exists today. So, how come MySQL is an interpreter language. See for yourself.

```
INSERT INTO users(first_name, last_name, email) VALUES('Jhon', 'Dice', 'jhon.dice@someone.com');
INSERT INTO users(first_name, last_name, email) ('Jhon' 'Douglas', 'jhon.douglas@someone.com');
INSERT INTO users(first_name, last_name, email) VALUES('Jhon', 'Broklen', 'jhon.broklen@someone.
```
What do you think will going to happen or the output will be. None of the records will get inserted or a record will get inserted and will produce an error on the second statement. If a record gets inserted then you can see by yourself the living example of an interpreter. Last but not the least, where does this interpreter thing come from. PHP which was known as PERSONAL HOME PAGE WAS AN INTERPRETER LANGUAGE. After Zend took over it became a complied language. Thanks!
