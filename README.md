# Project

// Done by Asmae-Ag   09/04/2022 18:33
// Thanks to Mobe for the mental support




#include<stdio.h>
#include<string.h>
#include<stdlib.h>



int ticket_number_n=999;
int ticket_number_u=-1;


typedef struct {
	char Name[30];
	int ticket_number;
}client_info;


typedef struct node{
	client_info element; 
	node* next;
}urgent_client_info;


typedef struct{
	urgent_client_info *front, *rear;
} queue;







// ************************** Functions **************************//
void menu();// menu to show all the options the client got.
void enqueue_Arr(client_info arr[], int *ticket_number_n, int *rear);
void dequeue_Arr(client_info arr[], int *rear);
void display_Arr(client_info arr[], int *rear);
urgent_client_info* Create_Node(urgent_client_info *new_node, int *ticket_number_u, char type, char *Transfer);
queue* Create_Queue(queue *q);
void enqueueLL(queue *q, urgent_client_info *new_node);
void dequeueLL(queue *q);
void displayLL(queue *q);

int Find(char *Name, client_info arr[], int rear);
void Trans(int Index_Emergency, int *rear, client_info *arr );
void msg_display(client_info *clients_queue, int rear);
// **************************************************************//


int main() {
	queue *q=Create_Queue(q);
	client_info clients_queue[10];
	urgent_client_info* head=NULL; 
	int choice=1, front=-1, rear=-1, Index_Emergency; 
	char Case, type;
	char Transfer[30];


while(choice!=5){
menu();
scanf("%d", &choice);

switch(choice){
	case 1: {
		printf("\tIs it a normal case or an emergency case? Enter N or E: ");
		getchar();
		scanf("%c", &Case);
		
			if(Case=='N'){
			enqueue_Arr(clients_queue, &ticket_number_n , &rear);
		
			}
			
			else if(Case=='E'){
				type='N';
				urgent_client_info * new_node= Create_Node(new_node,&ticket_number_u, type,Transfer);
				enqueueLL(q,new_node);
			}
		
		break;
	}
	
	case 2:{
		if(q->front!=NULL)
			dequeueLL(q);
		
		else 
			dequeue_Arr(clients_queue,&rear);	
			break;
}
	
	case 3:{
		printf("\tEnter which queue you want to display, answer with N or E: ");
		scanf(" %c",&Case);
		
		if(Case=='N')
			display_Arr(clients_queue,&rear);
			
		
		else if(Case=='E')
			displayLL(q);
			
		break;
	}
	
	case 4:{
		printf("\t\tEnter the client to transfer to the emergency department: ");
		getchar();
		gets(Transfer);
		Index_Emergency=Find(Transfer,clients_queue,rear);
	
		
		if(Index_Emergency==-2)
			printf("The queue is empty.");
			
		else if(Index_Emergency==-1)
			printf("The person does not exist in the queue");
		
		else{
			type='T';
		 	Trans(Index_Emergency,&rear,clients_queue);
		 	urgent_client_info * new_node= Create_Node(new_node,&ticket_number_u, type,Transfer);
		 	enqueueLL(q, new_node);
		}
		
		break;
} 
	 
	 case 5: {
	 	if (q->front==NULL){
	 		if(rear!=-1){
			 	msg_display(clients_queue, rear);
			 	exit;
			 }
		}
		else{
			while(q->front==NULL)
				dequeueLL(q);
				if(rear!=-1){
			 	msg_display(clients_queue, rear);
			 	exit;
		 	}
					
			}
			break;
}
			 
	 
		 
	 	

	
	default:
		printf("\t\tThis option does not exist, enter a valid option");
	
}	
}

return 0;
}
   
void menu(){
	printf("\t Please choose an option to make your next step\n \n");
	printf("\t \t1.Receive a New Client \n \n");
	printf("\t \t2.Serve Client \n\n");
	printf("\t \t3.Check a Queue Content\n\n");
	printf("\t \t4.Change the Status of a Client \n\n");
	printf("\t \t5.Service is OVER\n\n");
	printf("\t Your choice is: ");
}

// Function to enqueue elements in an array
void enqueue_Arr(client_info arr[], int *ticket_number_n, int *rear){
	
	if(*rear==9){
		printf("\t\tThe queue is full, you can not add another client for the time being\n");
	
	}
	
	else if(*rear==-1 || *rear<=8){
	
		printf("\t\tEnter the name of the client: ");
		getchar();
		gets(arr[++(*rear)].Name);
		(*ticket_number_n)++;
		arr[(*rear)].ticket_number= *ticket_number_n;
		
	}
		
	}
	
// Funtion to dequeue element in an array
void dequeue_Arr(client_info arr[], int *rear){
		if(*rear==-1){
			printf("\t\tThe queue is empty");
			printf("\n");
		}
		else if(*rear>=0 && *rear<=9){
			(*rear)--; 
			for(int i=1; i<=*rear; i++){
				arr[i+1]=arr[i];
			}
		}		
	}
	
// Function to create a node
urgent_client_info* Create_Node(urgent_client_info *new_node, int *ticket_number_u, char type, char Transfer[]){
		new_node=(urgent_client_info*)malloc(sizeof(urgent_client_info));
	if(type=='N'){
		printf("\t\tEnter the name of the client: "); 
		getchar();
		gets(new_node->element.Name);
	}
	
	else if(type=='T')
	strcpy(new_node->element.Name, Transfer);
	
		(*ticket_number_u)++;
		new_node->element.ticket_number= *ticket_number_u;
		new_node->next=NULL;
		return new_node;
	}
	
// Function to create a queue
queue* Create_Queue(queue *q) {
		q=(queue*)malloc(sizeof(queue));
		q->front=NULL;
		q->rear=NULL;
	
		return q; 
	}
	

	// Funtion to enqueue elements in the linked list
void enqueueLL(queue *q,urgent_client_info * new_node){


	 
	 if(q->rear==NULL){
	 	q->rear=new_node;
	 	q->front=new_node;
	 }
	 
	 else{
	 	q->rear->next=new_node;
	 	q->rear=new_node;
	 }
	 }


// Function to dequeue element in the linked list
void dequeueLL(queue *q){
	if(q->front==NULL){
		printf("\t\tThe queue is empty.");
		printf("\n");
	}
	
	else{
		urgent_client_info* temp=q->front;
		q->front=q->front->next;
		free(temp);
		
			if(q->front==NULL)
				q->rear=NULL;
		}
		}

// Funtion to display the content of the Array queue
void display_Arr(client_info arr[], int *rear){
	if(*rear==-1){
		printf("\t\tThe queue is empty");
		printf("\n");
	}
		

	
	else if(*rear>-1 && *rear<10){
		for(int i=0; i<=*rear; i++)
			printf("\t\tThe client name is %s with a ticket number %d\n",arr[i].Name,arr[i].ticket_number);
	}

}

// Function to display the content of the linked list queue
void displayLL(queue *q){
	urgent_client_info * walker=q->front;
	
	if (walker==NULL){
		printf("\t\tThe queue is empty");
		printf("\n");
	}

	
	while(walker!=NULL){
		printf("\t\tThe client name is %s with a ticket number %d", walker->element.Name, walker->element.ticket_number);
		printf("\n");
		walker=walker->next;
	}
	
		
	}
	
int Find(char *Name, client_info arr[], int rear){
	int i;
	if(rear==-1)
		return -2;
	
	
	if(rear>=0 && rear<=9){
		for(i=0;i<=rear; i++ )
			if(strcmp(Name,arr[i].Name)==0)
				break;
		
			else 
			return -1;
			}
			
	return i;	
	}
	
void Trans(int Index_Emergency, int *rear, client_info *arr ){
	
	if(*rear==Index_Emergency){
		(*rear)--;
	}
	
	else{
		for(int i=Index_Emergency; i<=*rear; i++){
		arr[i]=arr[i+1];
	}
	(*rear)--;
	}
		
}

void msg_display(client_info *clients_queue, int rear){
	 for(int i=0; i<=rear; i++){
	 	printf("Dear client %s ," ,clients_queue[i].Name);
	 }
	 printf("sorry to inform you that the doctor can receive another day.");
}

	
	
	
	
