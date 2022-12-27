/*#include "art.h"
int main()

{		
	Mainmenu();
	return 0;	
}*/

#define art_INCLUDED_
typedef struct Node
{
	char ID[30];// 用户名 
	char Sex[10];// 性别
	int Num;// 文章数 
	char Name[50];// 文章名 
	struct Node* next;// 指针域
}node;

node list;// 链表
#include "art.h"
#include<stdio.h>
#include<windows.h>
#include<stdlib.h>
#include<string.h>
// 读取文件
int Read(node* L);

// 保存文件
int  Save(node* L);
// 管理员菜单界面
void welcome();

// 添加用户信息
void Add(node *L,node q);
void Add();

// 删除用户信息
void Delete(node*L);
void Delete(node* s);

// 修改用户信息
void Fix(node *L);

// 查询用户信息
void Search(node* L);

// 输出用户信息
void print(node* L);


// 退出管理系统
void goodbye();

int main()
{
int choice = 0;
	while (true)
	{
		welcome();
		scanf("%d", &choice);
		switch (choice)
		{
		case 1:// 增加用户信息
			Add();
			break;
		case 2:// 删除用户信息
			Delete(&list);
			break;
		case 3:// 修改用户信息
			Fix(&list);
			break;
		case 4:// 查询用户信息
			Search(&list);
			break;
		case 5:// 输出用户信息
			print(&list);
			break;
		case 6:// 退出管理系统
			goodbye();
			break;
		}
		printf("是否需要继续操作？（Yes：1 / No：0)：");
		scanf("%d", &choice);
		if (choice == 0)
		{
			break;
		}
	}
	return 0;
}
void welcome()
{
	printf("****************************************************************\n");
	printf("***********             管理员菜单             ***********\n");
	printf("***********          1 ---- 增加用户信息             ***********\n");
	printf("***********          2 ---- 删除用户信息             ***********\n");
	printf("***********          3 ---- 修改用户信息             ***********\n");
	printf("***********          4 ---- 查询用户信息             ***********\n");
	printf("***********          5 ---- 输出用户信息             ***********\n");
	printf("***********          6 ---- 退出管理系统             ***********\n");
	printf("****************************************************************\n");

	printf("请选择想要实现的功能（数字）：");
}
// 增加用户信息
void Add()
{
system("cls");
	node user;
	printf("请输入添加用户的相关信息：\n");
	printf("用户名：");
	scanf("%s", &user.ID);
	printf("性别：");
	scanf("%s", user.Sex);
	printf("文章数：");
	scanf("%d", user.Num);
	printf("文章名：");
	scanf("%s", user.Name);
	

	Add(&list, user);

}
void Add(node* L, node q)
{
	node* p = L;   // 头插法
	node* s = (node*)malloc(sizeof(node));
	*s = q;

	s->next = p->next;
	p->next = s;

	Save(L);
}
// 删除学生信息
void Delete(node* L)
{
	system("cls");
	int id;

	node* p;

	printf("请输入要删除的用户的用户名：");
	scanf("%d", &id);
	node* user = Search(id, L);
	p = st;

	if (st == NULL)
	{
		printf("此用户不存在！\n");
		return;
	}

	user = user->next;
	printf("________________________________________________________________\n");
	printf("|用户名\t|性别\t|文章数\t|文章名\t|\n");
	printf("________________________________________________________________\n");
	printf("|%s\t|%s\t|%d\t|%s\t|\n", user->ID, user->Sex, user->Num, user->Name);
	printf("________________________________________________________________\n");

	Delete(p);
	// 保存信息
	Save_FILE(L);
}void Delete(node* s)
{
	node* t = s->next;

	s->next = t->next;
	t->next = NULL;

	free(t);
}
// 修改学生信息
void Fix(node* L){
}
// 查询学生信息
void Search(node* L){
}
// 输出学生信息
void Print(node* L){
}
