# Banker-sAlgorithm
/*Implementation of banker's algorithm using c
 author:Sunidhi Javali
 date:07-06-2021
 */
#include<stdio.h>
#include<stdlib.h>
//structure that describe resources
struct instances
{
   int a;
   int b;
   int c;
};
typedef struct instances instance;
//structure that describe process
struct process
{
    instance allocation;
    instance need;
    instance max;
    int flag;
};
void print(struct process[],int);
void readInfo(struct process[],instance*,int);
void bankersAlgorithm(struct process[],instance*,int);
int request(struct process[],instance*,instance*,int,int);
/*function name:Main
  parameters:void
  return type:int
  author:Sunidhi Javali
  date:07-06-2021
*/

int main()
{  
   int size;
   printf("-----------------------------------\n");
   printf("Welcome to Banker's Algorithm\n");
   printf("-----------------------------------\n");
   printf("wanna get safe sequence for following processes???...\n");
   printf("enter the following data\n");
   printf("ENTER THE TOTAL NUMBER OF PROCESSES INVOLVED\n");
   scanf("%d",&size);
    struct process p[size];
    instance available,reqInstance;
    int choice,reqProcess,permission;
    for(;;)
    { 
      readInfo(p,&available,size);
      printf("enter ur choice\n");
      printf("enter 1 for normal execution of banker's algorithm without any request\n");
      printf("enter 2 to request the resources and then form the safe sequence of processes\n");
      printf("enter 3 to exit\n");
      scanf("%d",&choice);
      switch(choice)
      {
         case 1 : bankersAlgorithm(p,&available,size);
                  break;
         case 2 : printf("enter the s.no of process which has requested for the resources\n");
                  scanf("%d",&reqProcess);
                  printf("enter the instances of each resource you want to request\n");
                  scanf("%d%d%d",&reqInstance.a,&reqInstance.b,&reqInstance.c);
                  permission=request(p,&available,&reqInstance,reqProcess,size);
                  if(permission==1)
                     bankersAlgorithm(p,&available,size);
                  else 
                     printf("request is out of range hence couldn't meet the process demand for resources\n");
                  break;
         case 3 : printf("thank you!!!!!!\n");
                  exit(0);
         default : printf("invalid choice\nchoose again!!!\n");
      }
    }  
 }
/*function name:readInfo
  parameters:process array,instances
  return type:void
  author:Sunidhi Javali
  date:07-06-2021
*/
void readInfo (struct process p[],instance *available,int size)
{   int i;
    printf("enter the total available sources of all instances\n");
    scanf("%d%d%d",&available->a,&available->b,&available->c);
    for(i=0;i<size;i++)
    {
       printf("enter the allocation for process %d\n",i);
       scanf("%d%d%d",&p[i].allocation.a,&p[i].allocation.b,&p[i].allocation.c);
       printf("enter the max accomodation of process %d\n",i);
       scanf("%d%d%d",&p[i].max.a,&p[i].max.b,&p[i].max.c);
       p[i].need.a = p[i].max.a - p[i].allocation.a;
       p[i].need.b = p[i].max.b - p[i].allocation.b;
       p[i].need.c = p[i].max.c - p[i].allocation.c;
       p[i].flag=0;
    }
    print(p,size);
    return;
}
/*function name:print
  parameters:process array
  return type:void
  author:Sunidhi Javali
  date:07-06-2021
*/
void print(struct process p[],int size)
{
   int i;
   printf("NEED MATRIX\na\tb\tc\n");
   for(i=0;i<size;i++)
      printf("%d\t%d\t%d\n",p[i].need.a,p[i].need.b,p[i].need.c);
   return;
}
/*function name:request
  parameters:process array,available instances,requested instances,requested process
  return type:int
  author:Sunidhi Javali
  date:07-06-2021
*/
int request(struct process p[],instance *available,instance *reqInstance,int reqProcess,int size)
{ 
   if((reqInstance->a<=available->a)&&(reqInstance->b<=available->b)&&(reqInstance->c<=available->c))
   {
      printf("Your request is been fulfilled\n");
      available->a=available->a-reqInstance->a;
      available->b=available->b-reqInstance->b;
      available->c=available->c-reqInstance->c;
      p[reqProcess].allocation.a=p[reqProcess].allocation.a+reqInstance->a;
      p[reqProcess].allocation.b= p[reqProcess].allocation.b+reqInstance->b;
      p[reqProcess].allocation.c= p[reqProcess].allocation.c+reqInstance->c;
      p[reqProcess].need.a=p[reqProcess].need.a-reqInstance->a;
      p[reqProcess].need.b=p[reqProcess].need.b-reqInstance->b;
      p[reqProcess].need.c=p[reqProcess].need.c-reqInstance->c;
      return 1;
   }
   return 0;
}
/*function name:bankersAlgorithm
  parameters:process array,available
  return type:void
  author:Sunidhi Javali
  date:07-06-2021
*/
void bankersAlgorithm(struct process p[],instance *available,int size)
{
   int i=0,j=0,count=0,safeSequence[size];
   while(count<size)
   {  if(j>3*size)
          break;
      if((p[i].need.a<=available->a)&&(p[i].need.b<=available->b)&&(p[i].need.c<=available->c)&&(p[i].flag==0))
      {
         available->a = available->a + p[i].allocation.a;
         available->b = available->b + p[i].allocation.b;
         available->c = available->c + p[i].allocation.c;
         printf("\n-----------------------------------\n");
         printf("\nprocess p%d is executed\ncurrent available resources are :\n",i);
         printf("%d\t%d\t%d",available->a,available->b,available->c);
         safeSequence[count]=i;
         count++;
         p[i].flag=1;
      }
      i=(i+1)%size;
      j++;
   }
   if(count==size)
   {
      printf("\n-----------------------------------\n");
      printf("\nsafe sequence is \n");
      for(i=0;i<size;i++)
         printf("process %d\n",safeSequence[i]);
   }
   else
     printf("\nsafe sequence for given set of processes cannot be generated\n");
   return;
}


    
