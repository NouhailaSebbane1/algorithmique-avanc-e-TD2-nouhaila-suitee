# algorithmique-avanc-e-TD2-nouhaila-suitee
voici l'implémentation de mes programmes demandés dans le TD 
TD 3 :
Exercice 1 (Creation d’une liste) :
/* Definition de la structure d’un element de la lsite */
struct Element{
int val;
struct Element *suivant;
};
typedef struct Element LISTE;
int InsererElementEnTete(LISTE **L, int valeur)
{
LISTE * element = NULL;
element = (LISTE*) malloc(sizeof(LISTE));
if (element == NULL){
printf("Probleme d’allocation memoire\n");
return 0;
}
element->val = valeur;
element->suivant = *L;
*L = element;
return 1;
}
La complexit ́e : O(1)

//Exercice 2 (Recherche de valeur) :
int RechercherValeur(LISTE *L, int valeur)
{
LISTE *ptr = L;
while (ptr){
if (ptr->val == valeur) return 1;
ptr = ptr->suivant;
}

//EXERCICE 3 
return 0;
}
Complexite : O(n)

int SuppressionValeurMin(LISTE **L)
{
LISTE * ptr = *L, *pMin=NULL, *pPrec = NULL;
int minim;
if (!ptr) /* liste vide */
return 0;
else if (!ptr->suivant) /* taille de liste = 1*/
{
*L = NULL;
free(ptr);
return 1;
}
minim = ptr->val; pMin = NULL;
while (ptr->suivant)/*chercher le min et sa position ds la liste*/
{
if (minim > ptr->suivant->val){
minim = ptr->suivant->val;
pMin = ptr;
}
ptr = ptr->suivant;
}
if (!pMin){
ptr = *L;
*L = (*L)->suivant;
free(ptr);
return 1;
} else{
ptr = pMin->suivant;
pMin->suivant = pMin->suivant->suivant;
free(ptr);
return 1;
}
}

//Exercice 4(Fusion de deux listes) :
Solution 1 :
LISTE * FusionDe2ListesAlternance(LISTE *L1, LISTE * L2)
{

// EX 5 

LISTE * ptr1 = L1, *ptr2 = L2, *res = NULL;
if (L1 == NULL) return L2;
else if (L2 == NULL) return L1;
while(ptr1 && ptr2)
{
InsererElementEnFin(&res,ptr1->val);
InsererElementEnFin(&res, ptr2->val);
ptr1 = ptr1->suivant;
ptr2 = ptr2->suivant;
}
while (ptr1)
{
InsererElementEnFin(&res, ptr1->val);
ptr1 = ptr1->suivant;
}
while (ptr2)
{
InsererElementEnFin(&res, ptr2->val);
ptr2 = ptr2->suivant;
}
return res;
}
Solution 2 :
LISTE * Fusion(LISTE *L1, LISTE * L2)
{
LISTE *p1 = L1, *p2 = L2, *p3, *p4;
if (!L1) return L2;
if (!L2) return L1;
LISTE * L = L1;
while (p1->suivant && p2->suivant)
{
p3 = p1->suivant;
p1->suivant = p2;
p4 = p2->suivant;
p2->suivant = p3;
p1 = p3;
p2 = p4;
}
if (p1->suivant == 0) p1->suivant = p2;
if (p2->suivant == 0) p2->suivant = p3;
return L;
}

void DestructionListe(LISTE **L)

//EXERCICE 6
{
LISTE *ptr;
if (*L == NULL) exit(0);
while (*L != NULL){
ptr = *L;
*L = (*L)->suivant;
free(ptr);
}
}


#include<stdio.h>
#include<stdlib.h>

typedef struct stack {
int data;
struct stack *next;
} STACK;
/* Empiler */
void push(STACK **head, int value){
/* create a new node */
STACK *node = NULL;
node = (STACK*)malloc(sizeof(STACK));
if (node == NULL)
{
fputs("Error: no space available for node\n", stderr);
abort();
} else {
/* initialisation */
node->data = value;
/* insertion*/
if (*head==NULL)
node->next = NULL;
else node->next = *head;
*head = node;
}
}
/* Depiler */
int pop(STACK **head)
{
int value;
if (*head==NULL) {/* pile est vide */
fputs("Error: stack underflow\n", stderr);


abort();
} else {/* depiler un element */
STACK *top = *head;
value = top->data;
*head = top->next;
free(top);
return value;
}
}

STACK * PairImpair(STACK * P1)
{
STACK * P2 = NULL, *P3 = NULL;
int val;
while (P1)
{
val = pop(&P1);
if (val % 2 == 0) push(&P2,val);
else push(&P3, val);
}
while (P3)
{
val = pop(&P3);
push(&P2,val);
}
return P2;
}
void AffichePile(STACK *P)
{
while (P)
{
printf("%d\t",P->data);
P = P->next;
}
printf("\n");
}
main()
{
int i;
STACK * P1 = NULL, *P2 = NULL;
for (i=1; i< 10; i++) push(&P1,i);
AffichePile(P1);
P2 = PairImpair(P1);
AffichePile(P2);
}

// Exercice 7 

/* structure de la pile */
typedef struct stack {
char data;
struct stack *next;
} STACK;

/* Empiler un caractere */
void push(STACK **head, char value){
/* create a new node */
STACK *node = NULL;
node = (STACK*)malloc(sizeof(STACK));
if (node == NULL)
{
fputs("Error: no space available for node\n", stderr);
abort();
} else {
/* initialisation */
node->data = value;
/* insertion*/
if (*head==NULL)
node->next = NULL;
else node->next = *head;
*head = node;
}
}
/* Depiler un caractere */
int pop(STACK **head)
{
char value;
if (*head==NULL) {/* pile est vide */
fputs("Error: stack underflow\n", stderr);
abort();
} else {/* depiler un element */
STACK *top = *head;
value = top->data;
*head = top->next;
free(top);
return value;
}
}

/* structure d’un element (noeud) de la file */
struct queue_node
{
struct queue_node *next;
char data;
};
/* structure de la file */
struct queue
{
struct queue_node *first;
struct queue_node *last;
};

/* Enfiler (rajouter) un element */
int enqueue(struct queue *q, const char value)
{
struct queue_node * node = NULL;
node = (struct queue_node *)malloc(sizeof(struct queue_node));
if (node == NULL) {
printf("probleme dallocation memoire");
return 1;
}
node->data = value;
if (q->first == NULL) {
q->first = q->last = node;
} else {
q->last->next = node;
q->last = node;
}
node->next = NULL;
return 0;
}
/* defiler (supprimer) un elment */
int dequeue(struct queue *q, char *value)
{
if (!q->first) {/* test si la file vide */
*value = 0;
return 1;
}
/* recuperer la valeur du noeud supprimer */
*value = q->first->data;
/* recuperer ladresse du noeud supprimer */
struct queue_node *tmp = q->first;
if (q->first == q->last) {
q->first = q->last = NULL;

} else {
q->first = q->first->next;
}
free(tmp); /* liberation de lesapce occupe par le noeud suprime*/
return 0;
}
/* la fonctin EcrireMessage */
struct queue * EcrireMessage(char chaine[])
{
int i,taille = strlen(chaine);
struct queue * q=NULL;
q = (struct queue *)malloc(sizeof(struct queue));
if (!q) { printf("Prob allocation mem "); exit(0);}
q->first = q->last = NULL;
for(i=0;i<taille;i++)
enqueue(q, chaine[i]);
return q;
}
/* Palindrome ? */
int EstPalindrome(char chaine[])
{
int t1 = strlen(chaine) % 2;
int t2 = strlen(chaine)/2;
int i;
char val, c1, c2;
STACK *p = NULL;
struct queue * Q;
Q = EcrireMessage(chaine);
for(i=0;i<t2; i++)
{
dequeue(Q,&val);
push(&p, val);
}
if (t1 != 0) dequeue(Q,&val);
for(i=0;i<t2;i++)
{
dequeue(Q,&c1);
c2 = pop(&p);
if (c1 != c2) return 0;
}
return 1;
}

