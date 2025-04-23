
Implementation of Lexical Analyzer in C /  Java / Python

import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter code: ");
        String code = sc.nextLine();

        Set<String> keywords = Set.of("float", "int", "char", "if", "else", "for", "while", "return");
        Set<String> ops = Set.of("+", "-", "*", "/", "=", "==");
        Set<String> delims = Set.of("(", ")", "{", "}", ";");

        String[] tokens = code.replace(";", " ;").split("\\s+");
        for (String token : tokens) {
            if (keywords.contains(token))
                System.out.println("Keyword: " + token);
            else if (ops.contains(token))
                System.out.println("Operator: " + token);
            else if (delims.contains(token))
                System.out.println("Delimiter: " + token);
            else if (token.matches("\\d+"))
                System.out.println("Integer: " + token);
            else if (token.matches("[a-zA-Z_][a-zA-Z0-9_]*"))
                System.out.println("Identifier: " + token);
            else
                System.out.println("Unknown: " + token);
        }
    }
}



NOT DONE:
Write a program to find FIRST & FOLLOW Symbols for the given grammar.



NOT DONE:
Implement Intermediate Code Generation using LEX and YACC

THREE ADDRESS CODE:
Three address code
Input: 
afc.l
%{
#include"y.tab.h"
%}
%%
[a-zA-Z]+ 	{strcpy(yylval.str,yytext);   return Var;}
[0-9]+		{strcpy(yylval.str,yytext);   return Num;}
\n		{return 0;}
[ \t]		{}
.		{return yytext[0];}
%%
int yywrap(){
return 1;
}     
afc.y
%{
#include<stdio.h>
#include<stdlib.h>
#include<string.h> 
char * createT();		// Declaration for creating the temporary variables
int tempcount=0;		// Global variable to track the number of temporary variables
%}
%union	{	char str[30];	} 
%left '+' 
%left '-'
%left '*'
%left '/'
%token <str> Var	// Defining the datatype of Tokens as str 
%token <str> Num
%type  <str> s		// Defining the datatypes of Non-Terminals
%type  <str> exp
%%
s	:	Var '=' exp	{printf("\n%s=%s\n",$1,$3);}
exp	:	'(' exp ')' 	{strcpy($$,$2);}
 	|	exp '+' exp	{strcpy($$,createT()); printf("\n%s=%s+%s",$$,$1,$3);} 
	|	exp '-' exp	{strcpy($$,createT()); printf("\n%s=%s-%s",$$,$1,$3);}
	|	exp '*' exp	{strcpy($$,createT()); printf("\n%s=%s*%s",$$,$1,$3);}
	|	exp '/' exp	{strcpy($$,createT()); printf("\n%s=%s/%s",$$,$1,$3);}
	|	Num  		{strcpy($$,$1);}
	|	Var		{strcpy($$,$1);}
;
%%
char * createT()
{
	char snum[30],*ptr;			
	sprintf(snum,"t%d",tempcount);	
	ptr=snum;			
	tempcount++;			
	return ptr;			
}
int main()
{	
yyparse(); 	
return 0;	
}
int yyerror(char *err)
{	
printf("\nInvlaid");
	exit(0);
}





Quad
quad.l
%{
#include"y.tab.h"
%}
%%
[a-zA-Z]+ 	{strcpy(yylval.str,yytext);   return Var;}
[0-9]+		{strcpy(yylval.str,yytext);   return Num;}
\n		{return 0;}
[ \t]		{}
.		{return yytext[0];}
%%
int yywrap(){
return 1;
}     

quad.y
%{
#include<stdio.h>
#include<stdlib.h>
#include<string.h> 
char * createT();		// Declaration for creating the temporary variables
int tempcount=0;		// Global variable to track the number of temporary variables
%}
%union	{	char str[30];	}
		// Redefining the datatype of yylval using union declaration, which is int by default 
%left '+' 
%left '-'
%left '*'
%left '/'
%token <str> Var	// Defining the datatype of Tokens as str 
%token <str> Num
%type  <str> s		// Defining the datatypes of Non-Terminals
%type  <str> exp
%%
s	:	Var '=' exp	{printf("\n%s=%s\n",$1,$3);}
exp	:	'(' exp ')' 	{strcpy($$,$2);}
 	|	exp '+' exp	{strcpy($$,createT()); printf("+ %s %s %s\n",$1,$3,$$);} 
	|	exp '-' exp	{strcpy($$,createT()); printf("- %s %s %s\n",$1,$3,$$);}
	|	exp '*' exp	{strcpy($$,createT()); printf("* %s %s %s\n",$1,$3,$$);}
	|	exp '/' exp	{strcpy($$,createT()); printf("/ %s %s %s\n",$1,$3,$$);}
	|	Num  		{strcpy($$,$1);}
	|	Var		{strcpy($$,$1);}
;
%%
char * createT()
{
	char snum[30],*ptr;			// Declaring the string array and pointer variable
	sprintf(snum,"t%d",tempcount);	// Returning a formatted String
	ptr=snum;			// Intializing the pointer with formatted string address
	tempcount++;				// Temporary count
	return ptr;				// Returning the pointer
}
int main()
{	
yyparse(); 	
return 0;	
}
int yyerror(char *err)
{	
printf("\nInvlaid");
	exit(0);
}


 
Parser Generator Tool : YACC ( The list may vary)
Simple Calculator
Cal.l:
%{
#include "y.tab.h"
%}

%%
[0-9]+ {yylval = atoi(yytext);return NUMBER;}
[ \t\n] {}
"+"  {return PLUS;}
"-"  {return MINUS;}
"*"  {return TIMES;}
"/"  {return DIVIDE;}
"("  {return R_P;}
")"  {return L_P;}
";"  {return END;}
. { }
%%
int yywrap(void) {return 1;}

Cal.y:
%{
#include<stdio.h>
%}

%token NUMBER
%token PLUS MINUS TIMES DIVIDE
%token LEFT_PARENTHESIS RIGHT_PARENTHESIS
%token END

%left PLUS MINUS
%left TIMES DIVIDE
%%

lines : | lines line;
line : exp END {printf("answer : %d" , $1) ;};
exp : term {$$ = $1;} | exp PLUS term{$$ = $1 + $3;} | exp MINUS term{$$ = $1 - $3;};
term : factor {$$ = $1;} | term TIMES factor{$$ = $1 * $3;} | term DIVIDE factor{$$ = $1 / $3;};
factor : NUMBER{$$ = $1;} | LEFT_PARENTHESIS exp RIGHT_PARENTHESIS {$$ = $2;};
%%

int main(void){
	printf("exp : ");
	return yyparse();
}
int yyerror(char *s){
	fprintf(stderr , "%s\n",s);
}

Recognize nested IF statement and display levels
Nested.l
%{
#include "y.tab.h"
%}

%%
"if"            { return IF; }
"else"          { return ELSE; }
"{"             { return LBRACE; }
"}"             { return RBRACE; }
[ \t\n]+        ;        
.               ;         
%%
int yywrap(void){ return 1;}

Nested.y
%{
#include <stdio.h>
int nesting_level = 0;
int max_level = 0;
void yyerror(const char *s);
int yylex(void);
%}

%token IF ELSE LBRACE RBRACE

%%
program:  statements  {printf("Maximum nesting level: %d\n", max_level);  }  ;
statements:  | statements statement  ;
statement: IF_BLOCK  ;
IF_BLOCK: IF_PART  | IF_PART ELSE IF_BLOCK  ;
IF_PART:  IF LBRACE {   nesting_level++; if (nesting_level > max_level) max_level=nesting_level;}  statements RBRACE { nesting_level--;}  ;
%%

int main() {
	printf("Statement : ");
	return yyparse();
}
void yyerror(const char *s) {
    fprintf(stderr, "Error: %s\n", s);
}

Program to recognize a valid variable in C language

Var.l
%{
#include "y.tab.h"
%}
%%
[a-zA-Z_][a-zA-Z0-9_]*   { return IDENTIFIER; } 
[ \t\n]+                 {}
.                        { return yytext[0]; } 
%%
int yywrap() { return 1; }

var.y
%{
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
extern char *yytext; 
int is_keyword(char *name);
%}
%token IDENTIFIER
%%
stmt: IDENTIFIER { 
    if (is_keyword(yytext)) 
        printf("Invalid: '%s' is a C keyword\n", yytext);
    else 
        printf("Valid variable: %s\n", yytext); 
}
;
%%
int is_keyword(char *name) {
    char *keywords[] = {
        "int", "return", "while", "if", "else", "for", "float", "double", "char", "void",
        "switch", "case", "break", "continue", "typedef", "struct", "union", "default",
        "do", "enum", "extern", "goto", "register", "short", "signed", "sizeof",
        "static", "volatile", "const", "long", "unsigned", "auto", "inline", "restrict",
        "_Bool", "_Complex", "_Imaginary"
    };
    int i;
    for (i = 0; i < sizeof(keywords) / sizeof(keywords[0]); i++) {
        if (strcmp(name, keywords[i]) == 0) 
            return 1;
    }
    return 0;
}


int main() {
    printf("Enter a variable name: ");
    yyparse();
    return 0;
}
int yyerror(char *s) {
    printf("Invalid variable name\n");
    return 1;
}


Implement Lexical Analyzer using FLEX( The list may vary)
Count no of Vowels & Consonants
%{
#include<stdio.h>
int v=0,c=0;
%}

%%
[aeiouAEIOU] {v++;}
[a-zA-Z] {c++;}
. { }
%%

int yywrap(void) {return 1;}
int main(){
printf("text: ");
yylex();
printf("%d vowels and %d conso",v,c);
return 0;}

Count no of Words, characters & lines
%%
[a-zA-Z]+     {w++;  c += strlen(yytext);}
[a-zA-Z^\t]           {c++;}
\n                  {l++;}
%%

Count no of  keywords, identifiers & operators
%{
#include<stdio.h>
#include<stdlib.h>
int kp=0 , op=0 , constants=0 , id=0 , punctuators=0;
%}

%%
"int"|"float"|"double"|"long"|"return"|"if"|"else"|"void"|"for" {kp++;}
"+"|"-"|"/"|"*"|"=="|"<="|">=" {op++;}
[a-zA-Z_][a-zA-Z0-9_]* {id++;}
[0-9]+ {constants++;}
"{"|"}"|"("|")"|"["|"]" {punctuators;}
[\t\n]+ { }
. { }
%%


int yywrap(void){return 1;}
int main(){
	FILE *file = fopen("input.c","r");
	if(file == NULL) {
		printf("no file");
		return 0;
	}
yyin=file;
	yylex();
	printf("keywords = %d\n operators = %d\n constants = %d\n identifiers = %d\n punctuators = %d\n",kp,op,constants,id,punctuators);
	return 0;
}

Identify Even & odd integers
int num;
%}
%%
[0-9]+ {
	num = atoi(yytext);
	if(num%2==0) printf("%d is even number \n",num);
	else printf("%d odd num\n");
}
%%

Count of printf & scanf statements in C program
Random .c file:
#include <stdio.h>
int main() {
    int n1, n2, sum;
    printf("Enter two integers: ");
    scanf("%d %d", &n1, &n2);
    sum = n1 + n2;
    printf("Sum: %d",sum);
    return 0;
}
.l file:
%{
#include <stdio.h>
#include <stdlib.h>
int pc = 0;
int sc = 0;
%}
%%
printf   { pc++;}
scanf  { sc++;}
.|\n   { }
%%
int yywrap(void) {return 1;}
int main() {
	FILE *file = fopen("input.c", "r"); 
	if (file == NULL) {
		perror("Error opening file");
		return 1;
	}
	yyin = file;
	yylex();
	printf("Count of printf statements: %d\n", pc);
	printf("Count of scanf statements: %d\n", sc);
	fclose(file);
	return 0;
}

Classify English words as verbs, adverbs, adjectives etc..
%{
#include<stdio.h>
#include<string.h>
void classify(char *word);
%}
%%
[a-zA-Z]+ {classify(yytext);}
[ \t\n\r]+ { }
. {printf("Others %s\n",yytext);}
%%
int yywrap(void) {return 1;}
void classify (char *word){
	const char *noun[] = {"school" , "teacher" , "yard" , "student" , "building" , "classroom" , "house" , "palace" ,"ball" , NULL};
	const char *verb[] = {"teaches" , "studies" , "goes" , "chases" , NULL};
	const char *adjective[] = {"high" , "tall" , "small" , "black" , "short" , "bright" , NULL};

	int i;
	for( i=0;noun[i] != NULL; i++)
		if( (strcmp(word , noun[i])) == 0 ){
			printf("NOUN : %s\n",word);
			return;
		}
	for( i=0;verb[i] != NULL; i++)
		if( (strcmp(word , verb[i])) == 0 ){
			printf("VERB : %s\n",word);
			return;
		}
	for( i=0;adjective[i] != NULL; i++)
		if( (strcmp(word , adjective[i])) == 0 ){
			printf("ADJECTIVE : %s\n",word);
			return;
		}
}

int main(){
	char input[256];
	printf("Sentence : ");
	fgets(input , sizeof(input) , stdin);
	YY_BUFFER_STATE buffer = yy_scan_string(input);
	yylex();
	yy_delete_buffer(buffer);
	return 0;
}

