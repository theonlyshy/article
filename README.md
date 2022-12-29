#include<stdio.h>
#include<windows.h>
#include<stdlib.h>
#include<string.h>
typedef struct Node
{
	int ID;// 用户名 
	char Sex[10];// 性别
    int Num;// 文章数 
	char Name[50];// 文章名 

	struct Node* next;// 指针域
}node;

node list;// 链表

// 读取文件
int Read_FILE(node* L);

// 保存文件
int  Save_FILE(node* L);

// 主菜单界面
void managermenu();

// 增加用户信息
void Add(node *L,node e);
void Add_Printf();

// 删除用户信息
void Delete_Printf(node*L);
void Delete(node* s);

// 修改用户信息
void Fix(node *L);

// 查询用户信息
void Search_Printf(node* L);
node* Search_id(int id, node* L);// 按用户名进行查找
node* Search_name(char name[], node* L);// 按文章名进行查找

// 输出用户信息
void Print(node* L);
void Print_Printf();



int main()
{
	
	int choice = 0;
	Read_FILE(&list);
	while (true)
	{
		managermenu();
		scanf("%d", &choice);
		switch (choice)
		{
		case 1:// 增加用户信息
			Add_Printf();
			break;
		case 2:// 删除用户信息
			Delete_Printf(&list);
			break;
		case 3:// 修改用户信息
			Fix(&list);
			break;
		case 4:// 查询用户信息
			Search_Printf(&list);
			break;
		case 5:// 输出用户信息
			Print(&list);
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

void managermenu()
{
	system("cls");
	printf("****************************************************************\n");
	printf("***********            管理员菜单            ***********\n");
	printf("***********          1 ---- 增加用户信息             ***********\n");
	printf("***********          2 ---- 删除用户信息             ***********\n");
	printf("***********          3 ---- 修改用户信息             ***********\n");
	printf("***********          4 ---- 查询用户信息             ***********\n");
	printf("***********          5 ---- 输出用户信息             ***********\n");
	printf("****************************************************************\n");

	printf("请选择想要实现的功能（数字）：");
}

// 读取文件
int Read_FILE(node* L)
{
	FILE* pfRead = fopen("student_information.txt", "q");
	node user;
	node* s;
	node* t = L;

	if (pfRead == NULL)
	{
		return 0;
	}

	while (fscanf(pfRead, "%d %s %d %s", &user.ID, user.Sex, &user.Num, user.Name) != EOF)
	{
		s = (node*)malloc(sizeof(node));

		*s = user;

		// 尾插法
		t->next = s;
		t = s;
		t->next = NULL;
	}

	return 1;
}

// 保存文件
int  Save_FILE(node* L)
{
	FILE* pfWrite = fopen("student_information.txt", "p");
	if (pfWrite == NULL)
	{
		return 0;
	}

	node* p = L->next;

	while (p != NULL)
	{
		fprintf(pfWrite, " %d %s %d %s\n", p->ID, p->Sex, p->Num, p->Name);
		p = p->next;
	}

	return 1;
}

// 增加用户信息
void Add_Printf()
{
	system("cls");
	node user;
	printf("请输入添加用户的相关信息：\n");
	printf("用户名：");
	scanf("%d", &user.ID);
	printf("性别：");
	scanf("%s", user.Sex);
	printf("文章数：");
	scanf("%d", user.Num);
	printf("文章名：");
	scanf("%s", user.Name);
	Add(&list, user);
}

void Add(node* L, node e)
{
	// 头插法
	node* p = L;
	node* s = (node*)malloc(sizeof(node));
	*s = e;

	s->next = p->next;
	p->next = s;

	Save_FILE(L);
}

// 删除用户信息
void Delete_Printf(node* L)
{
	system("cls");
	int id;

	node* p;

	printf("请输入要删除的用户的用户名：");
	scanf("%d", &id);
	node* user = Search_id(id, L);
	p = user;

	if (user == NULL)
	{
		printf("此用户不存在！\n");
		return;
	}

	user = user->next;
	printf("________________________________________________________________\n");
	printf("|用户名\t|性别\t|文章数\t|文章名\t|\n");
	printf("________________________________________________________________\n");
	printf("|%d\t|%s\t|%d\t|%s\t|\n", user->ID, user->Sex, user->Num, user->Name);
	printf("________________________________________________________________\n");

	Delete(p);
	// 保存信息
	Save_FILE(L);
}

void Delete(node* s)
{
	node* t = s->next;

	s->next = t->next;
	t->next = NULL;

	free(t);
}

// 修改用户信息
void Fix(node* L)
{
	system("cls");
	int id;
	printf("请输入要修改的用户的用户名：");
	scanf("%d", &id);
	node* user = Search_id(id, L);

	if (user == NULL)
	{
		printf("此用户不存在！\n");
		return;
	}

	user = user->next;
	int choice = 0;
	while (1)
	{
		system("cls");

	// 输出一次所要修改的用户信息 
		printf("________________________________________________________________\n");
		printf("|用户名\t|性别\t|文章数\t|文章名\t|\n");
		printf("________________________________________________________________\n");
		printf("|%d\t|%s\t|%d\t|%s\t|\n", user->ID, user->Sex, user->Num, user->Name);
		printf("________________________________________________________________\n");

		printf("修改用户名 ---- 1\n");
		printf("修改性别 ---- 2\n");
		printf("修改文章数 ---- 3\n");
		printf("修改文章名 ---- 4\n");

		printf("请输入要修改的信息：");
		scanf("%d", &choice);

		switch (choice)
		{
		case 1:
			printf("请输入用户名："); 
			scanf("%d", user->ID);
			break;
		case 2:
			printf("请输入性别：");
			scanf("%s", user->Sex);
			break;
		case 3:
			printf("请输入文章数：");
			scanf("%d", user->Num);
			break; 
		case 4:
			printf("请输入文章名：");
			scanf("%s", user->Name);
			break;
		}
		printf("是否继续修改该用户信息？（Yes：1 / No：0）：");
		scanf("%d", &choice);
		if (choice == 0)
		{
			break;
		}
	}

	// 修改完成后该用户的信息
	printf("________________________________________________________________\n");
	printf("|用户名\t|性别\t|文章数\t|文章名\t|\n");
	printf("________________________________________________________________\n");
	printf("|%d\t|%s\t|%d\t|%s\t|\n", user->ID, user->Sex, user->Num, user->Name);
	printf("________________________________________________________________\n");

	// 保存信息
	Save_FILE(L);
}

// 查询用户信息
void Search_Printf(node* L){
	system("cls");
	int choice = 0;
	printf("按照用户名查询 ---- 1\n");
	printf("按照文章名查询 ---- 2\n");
	printf("请输入查询方式：");
	scanf("%d", &choice);

	int id;
	char name[50];
	node* user;
	if (choice == 1)
	{
		printf("请输入要查询的用户名：");
		scanf("%d", &id);
		user = Search_id(id, L);

		if (user == NULL)
		{
			printf("此用户不存在！\n");
		}
		else
		{
		user = user->next;
			printf("________________________________________________________________\n");
			printf("|用户名\t|性别\t|文章数\t|文章名\t|\n");
			printf("________________________________________________________________\n");
			printf("|%d\t|%s\t|%d\t|%s\t|\n", user->ID, user->Sex, user->Num, user->Name);
			printf("________________________________________________________________\n");
		}
	}
	else if (choice == 2)
	{
		printf("请输入要查询的文章名：");
		scanf("%s", name);
		user = Search_name(name, L);

		if (user == NULL)
		{
			printf("此用户不存在！\n");
		}
		else
		{
			user = user->next;
			printf("________________________________________________________________\n");
			printf("|用户名\t|性别\t|文章数\t|文章名\t|\n");
			printf("________________________________________________________________\n");
			printf("|%d\t|%s\t|%d\t|%s\t|\n", user->ID, user->Sex, user->Num, user->Name);
			printf("________________________________________________________________\n");
		}
	}
}

// 按用户名进行查找
node* Search_id(int id, node* L)
{
	node* p = L;
	
	while (p->next != NULL)
	{
		if (p->next->ID == id)
		{
			return p;
		}
		p = p->next;
	}
	return NULL;
}

// 按文章名进行查找
node* Search_name(char name[], node* L)
{
	node* p = L;

	while (p->next != NULL)
	{
		if (strcmp(name, p->next->Name) == 0)
		{
			return p;
		}
		p = p->next;
	}
	return NULL;
}

// 输出用户信息
void Print(node* L)
{
	system("cls");
	node* p = L->next;
	Print_Printf();
	if (p != NULL)
	{
		while (p != NULL)
		{
			printf("________________________________________________________________\n");
			printf("|%d\t|%s\t|%d\t|%s\t|\n", p->ID, p->Sex, p->Num, p->Name);
			printf("________________________________________________________________\n");
			p = p->next;
		}
	}
}

void Print_Printf()
{
	printf("________________________________________________________________\n");
	printf("|用户名\t|性别\t|文章数\t|文章名\t|\n");
	printf("________________________________________________________________\n");
}




