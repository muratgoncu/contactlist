contactlist
===========
/*
 * contactlist.c
 */

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "contactlist.h"

void my_getline(FILE *stream, char *s, int size) {
	fgets(s, size, stream);
	char *p = strchr(s, '\n');
	if (p) {
		*p = '\0';
	}
}

void print_contact(struct Contact* contact) {
        printf("Name: %32s, Phone: %16s\n", contact->name, contact->phone);

}

/* TODO */
void insert_new_contact(struct Contact **contact_list, char *name, char *phone) {
    //printf("%s", name);
    //printf("%s \n", phone);
    struct Contact *new_contact = malloc(sizeof(struct Contact)) ;
    did_allocate(new_contact);
    memcpy(new_contact->name, name, sizeof(new_contact->name));
    printf("%s ",new_contact->name);
    
    memcpy(new_contact->phone, phone, sizeof(new_contact->phone));
     printf("%s\n",new_contact->phone);
    if((*contact_list) == NULL){
        (*contact_list) = new_contact;
        printf("NULL\n");
        (*contact_list)->next = NULL;
        print_contact_list(*contact_list);
    }
    else{
        struct Contact *temp = NULL ;
        //did_allocate(temp);
        struct Contact *temp2 = NULL ;
        //did_allocate(temp2);

        temp2=(*contact_list);
        int comp_res = strcasecmp(name, (*contact_list)->name);
        //ilk elemandan onceye konacak ise
        if(comp_res < 0){
        	new_contact->next=(*contact_list);
        	temp2=new_contact;
            printf("1\n");
            print_contact_list(*contact_list);
        }
        else{
           
            while((*contact_list) != NULL && comp_res > 0){
                temp = (*contact_list);
                (*contact_list) = (*contact_list)->next;
                if((*contact_list != NULL)){
                comp_res = strcasecmp(name, (*contact_list)->name);
                    printf("2\n");
                }
            }
            if((*contact_list) == NULL) {//sonuncu eleman ise
            	(*contact_list) = temp;
            	(*contact_list)->next = new_contact;
            	new_contact->next = NULL;
                printf("3\n");
            }
            else{
            	new_contact->next = temp->next;
            	(*contact_list)=temp;
            	(*contact_list)->next=new_contact;
                printf("4\n");
                print_contact_list(*contact_list);
            }
        }
        (*contact_list)=temp2;
        printf("\n");
        print_contact_list(*contact_list);
    }
}

/* TODO */
struct Contact* load_contact_list(char *filename) {

    
    FILE *f = fopen(filename, "r+");
    struct Contact *cl = malloc(sizeof(struct Contact));
    if(f == NULL){
        return cl=NULL;
    }
    else{
        if(cl == NULL){
            printf(" Couldn't allocate memory for contact list (Function load contact list)");
            exit(1);
        }
        char line[256] , name1[32], *name2, phone1[16], *phone2;
        cl = NULL;
        while(!feof(f)){
            
            my_getline(f,line,(sizeof(cl->name)+sizeof(cl->phone))+2);
            name2 = strtok(line, ",");
            phone2 = strtok(NULL, ",");
            if(phone2 == NULL){
                break;
            }
            //strcpy(phone1, phone2);
            memcpy(name1, name2, sizeof(name1));
            memcpy(phone1, phone2, sizeof(phone1));
            //printf("%s ",name1);
            //printf("%s \n",phone1);
            insert_new_contact(&cl,name1,phone1);
        }
        fclose(f);
        return cl;
    }

}

/* TODO */
void save_contact_list(struct Contact *contact_list, char *filename) {
    
    FILE *f = fopen(filename, "w+");
    if(f==NULL){
        
        printf("Couldn't open file");
        exit(1);
    }
    else{
        
        while(contact_list != NULL){
            
            fprintf(f,"%s,%s\n",contact_list->name,contact_list->phone);
            contact_list = contact_list->next;
        }
    }
                fclose(f);
}

/* TODO */
void modify_contact(struct Contact *contact_list, char *which_name, char *new_phone) {

    while(contact_list != NULL && strcasecmp(which_name, contact_list->name)!=0 ){
        contact_list = contact_list->next;
    }
    if(contact_list == NULL)
    	printf("no person available");
    else
    	memcpy(contact_list->phone, new_phone, sizeof(contact_list->phone));
}

/* TODO */
void delete_contact(struct Contact **contact_list, char *which_name) {
    struct Contact *temp = NULL;

    if(strcasecmp(which_name, (*contact_list)->name) == 0){
        
        temp = *contact_list;
        *contact_list = temp->next;
        free(temp);
    }
    else{
        
        struct Contact *temp2 = NULL;
        temp2 = *contact_list;

        while((*contact_list) != NULL && strcasecmp(which_name, (*contact_list)->name) != 0){
        
            temp=(*contact_list);
            (*contact_list)=(*contact_list)->next;
        }
		
        *contact_list = temp;
		(*contact_list)->next=(*contact_list)->next->next;
		(*contact_list)->next = NULL;
		*contact_list=temp2;
    }
}

/* TODO */
void print_contact_list(struct Contact *contact_list) {
	if(contact_list == NULL)
		printf("Contact list is empty");
	else if(contact_list->next == NULL)
			print_contact(contact_list);
	else{
			do{
				print_contact(contact_list);
		}while((contact_list = contact_list->next) != NULL);
	}
}

/* TODO */
void free_list(struct Contact *contact_list) {
    struct Contact *temp;
    while(contact_list != NULL){
        temp = contact_list;
        contact_list = contact_list->next;
        free(temp);
    }
}

/* TODO (2.kisim) */
void generate_graph(struct Contact *contact_list) {
}

void did_allocate(struct Contact *contact_list){
    if(contact_list == NULL){
        printf("Couldn't allocate memory");
        exit(1);
    }
}



proje
