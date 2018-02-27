#include <iostream>
#include <stdlib.h>
#include <time.h>
#include <vector>
#include <random>
#include <queue>
#include <fstream>
#include <string>
#include <algorithm>

using namespace std;

template <class T>
struct node
{
	T inf;
	node *left, *right;
};

template <class T>
class Tree
{
private:

	node<T> *root;
	queue<node<T>*> rootQueue;
	vector<string> fights;

	void CopyTree(Tree const& other)
	{
		cpy(root, other.root);
	}

	void cpy(node<T>*& curr, node<T>* const& other)
	{
		curr = NULL;
		if (other)
		{
			curr = new node<T>;
			curr->inf = other->inf;
			cpy(curr->left, other->left);
			cpy(curr->right, other->right);
		}
	}

	void DeleteTree(node<T>*& other)
	{
		if (other)
		{
			DeleteTree(other->left);
			DeleteTree(other->right);
			delete other;
			other = NULL;
		}
	}

	void LevelOrder(node<T>* const& input)
	{
		queue<node<T>*> q;
		q.push(input);

		while (!q.empty())
		{
			node<T>* temp = q.front();
			q.pop();

			cout << temp->inf << " ";

			if (temp->left)
				q.push(temp->left);

			if (temp->right)
				q.push(temp->right);
		}
		cout << endl;
	}

public:

	Tree()
	{
		root = NULL;
	}

	Tree(Tree const& other)
	{
		CopyTree(other);
	}

	Tree const& operator = (Tree const& other)
	{
		if (this != &other)
		{
			DeleteTree(root);
			CopyTree(other);
		}
		return *this;
	}

	~Tree()
	{
		DeleteTree(root);
	}

	bool Empty() const
	{
		return root == NULL;
	}

	node<T>* GetRoot() const
	{
		return root;
	}

	T RootTree() const
	{
		return root->inf;
	}

	Tree LeftTree() const
	{
		Tree<T> t;
		cpy(t.root, root->left);
		return t;
	}

	Tree RightTree() const
	{
		Tree<T> t;
		cpy(t.root, root->right);
		return t;
	}

	void Print()
	{
		LevelOrder(root);
		cout << endl;
	}

	void Create()
	{
		char c;
		T data;
		do {
			cout << "Root: "; cin >> data;
			NewElement(data);
			cout << "New root y/n?"; cin >> c;
		} while (c == 'y');
	}

	void NewElement(T data)
	{
		node<T>* temp;
		temp = new node<T>;
		temp->inf = data;
		temp->left = NULL;
		temp->right = NULL;

		bool completed = false;

		if (Empty())
			root = temp;
		else
		{
			do
			{
				node<T>* fr = rootQueue.front();

				if (!fr->left)
				{
					fr->left = temp;
					completed = true;
				}
				else if (!fr->right)
				{
					fr->right = temp;
					completed = true;
					rootQueue.pop();
				}
				else if (fr->left && fr->right)
					rootQueue.pop();
			} while (completed == false);
		}
		rootQueue.push(temp);
	}

	void FillTree(vector<T> leaves)
	{
		int layers = 0;
		unsigned int a = 1;

		while (a < leaves.size())
		{
			a *= 2;
			layers++;
		}

		a = 1;
		T emptyElement = T();

		for (int i = 0; i < layers; i++)
		{
			for (unsigned int j = 0; j < a; j++)
			{
				NewElement(emptyElement);
			}

			a *= 2;
		}

		srand(time(0));

		for (int i = 0; i <= rand() % 10; i++)
			random_shuffle(leaves.begin(), leaves.end());

		for (unsigned int i = 0; i < leaves.size(); i++)
		{
			NewElement(leaves[i]);
		}
	}

	vector<string> GetParticipants()
	{
		ifstream participantsFile;
		participantsFile.open("participants.txt", ios::in);
		vector<string> vectorParticipants;

		if (!participantsFile)
		{
			cerr << "File couldn't be opened!" << endl;
			exit(1);
		}

		int numLines = 0;
		string participantName;
		string participantGrade;

		while (participantsFile >> participantName >> participantGrade)
		{
			vectorParticipants.push_back(participantName + " " + participantGrade);
			numLines++;
		}

		srand(time(0));
		for (int i = 0; i <= rand() % 10; i++)
			random_shuffle(vectorParticipants.begin(), vectorParticipants.end());

		for (int i = 0; i < rand() % numLines; i++)
			vectorParticipants.pop_back();

		participantsFile.close();

		return vectorParticipants;
	}

	void Tournament()
	{
		if (root == NULL)
		{
			cout << "No participants in tournament!" << endl;
			return;
		}

		if (root->left == NULL)
		{
			cout << "The winner is " << root->inf << endl;
			return;
		}

		while(root->inf == T())
			TournamentIterate(root);
	}

	void TournamentIterate(node<T>*& n)
	{
		if (n->left && n->left->inf != T())
		{
			if (n->right && n->right->inf != T())
			{
				node<T>* winner;
				winner = DetermineWinner(n->left, n->right);
				if (winner == n->left)
					n->inf = n->left->inf;
				else
					n->inf = n->right->inf;
			}
			else
				n->inf = n->left->inf;

			return;
		}

		if(n->left)
			TournamentIterate(n->left);
		if(n->right)
			TournamentIterate(n->right);
	}

	node<T>* DetermineWinner(node<T>* const& part1, node<T>* const& part2)
	{
		int skill1, skill2, pos1, pos2;
		string name1, name2;

		pos1 = part1->inf.find(" ") + 1;
		pos2 = part2->inf.find(" ") + 1;
		skill1 = stoi(part1->inf.substr(pos1));
		skill2 = stoi(part2->inf.substr(pos2));

		name1 = part1->inf.substr(0, pos1 - 1);
		name2 = part2->inf.substr(0, pos2 - 1);

		fights.push_back(name1 + " VS " + name2);

		srand(time(0));

		int randomBall = rand() % (skill1 + skill2);

		if (randomBall >= 0 && randomBall <= skill1 - 1)
			return part1;
		else
			return part2;
	}

	void findpath(string name)
	{
		if (fights.empty())
		{
			cout << "No participants in tournament!" << endl;
			return;
		}

		bool found = false;

		for (int i = 0; i < fights.size(); i++)
		{
			string f = fights[i];

			int pos = f.find(" ");
			string n1 = f.substr(0, pos);
			string n2 = f.substr(pos + 4);

			if (n1 == name || n2 == name)
			{
				cout << n1 << " VS " << n2 << endl;
				found = true;
			}
		}

		if (!found)
		{
			cout << "No participant with name " << name << "!" << endl;
		}
	}

	void allFights()
	{
		for (int i = 0; i < fights.size(); i++)
		{
			cout << fights[i] << endl;
		}

		cout << "The winner is " << root->inf << endl;
	}
};
