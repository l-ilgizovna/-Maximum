#include <stdio.h>
#include <stdlib.h>
#define Elem struct Element
#define Stack struct stack
#define nel 100000

struct Element { 
        int elem, max; 
};

struct stack { 
	Elem *data;
	int cap, top1, top2; 
};

Stack Init() {
	Stack s;
	s.data=(Elem*)malloc(nel*sizeof(Elem));
	s.cap=nel;
	s.top1=0;
	s.top2=nel-1;
	return s;
}

int Empty1 (Stack *s) {
	return (s->top1==0);
}

int Empty2 (Stack *s) {
	return (s->top2==s->cap-1);
}

int Empty (Stack *s) {
	return (Empty1(s)&&Empty2(s));
}

void ENQ (Stack *s, int x) { 
	s->data[s->top1].elem=x;
	if (!Empty1(s) && (x<s->data[s->top1-1].max)) s->data[s->top1].max=s->data[s->top1-1].max;
	else s->data[s->top1].max=x;
	s->top1++;

}

int DEQ (Stack *s) { 
	if (Empty2(s))
		while (s->top1>0) {
			s->top1--;
			s->data[s->top2].elem=s->data[s->top1].elem;
			if (!Empty2(s) && (s->data[s->top2+1].max > s->data[s->top1].elem)) s->data[s->top2].max=s->data[s->top2+1].max;
			else s->data[s->top2].max=s->data[s->top1].elem;
			s->top2--;
		}
	s->top2++;
	return s->data[s->top2].elem;
}


int MAX (Stack s) { 
	if (!Empty1(&s) && (Empty2(&s) || (Empty2(&s) && (s.data[s.top1-1].max>s.data[s.top2+1].max)))) return s.data[s.top1-1].max;
	else return s.data[s.top2+1].max;
}

void main () {
	int n,i,x;
	Stack q;
	q=Init();
	scanf("%d",&n);
	char *kom=(char*)malloc(6*sizeof(char));
	for (i=0;i<n;i++) {
		scanf("%s",kom);
		if (!strcmp(kom,"EMPTY")) {
			if (Empty(&q)) printf("true\n");
			else printf("false\n");
			continue;
		}
		if (!strcmp(kom,"ENQ")) {
			scanf("%d",&x);
			ENQ(&q,x);
			continue;
		}
		if (!strcmp(kom,"DEQ")) {
			x=DEQ(&q);
			printf("%d\n",x);
			continue;
		}
		if (!strcmp(kom,"MAX")) {
			x=MAX(q);
			printf("%d\n",x);
			continue;
		}
	}
	free(kom);
	free(q.data);
}
