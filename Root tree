#include <iostream>
#include <queue>
#include <vector>
#include <fstream>
#include <string>
#include <algorithm>
#include <ctime>
#include <cstdlib>

using namespace std;

template <class T>
class RootTree
{
private:
	T root;
	RootTree<T>* left;
	RootTree<T>* right;
	queue<RootTree<T> > rootQueue;
public:
	void CopyTree(RootTree<T> const& other)
	{
		root = other.root;
		*left = *other.left;
		*right = *other.right;
	}

	void DeleteTree()
	{
		root = 0;
		delete left;
		delete right;
	}

	RootTree()
	{
		root = 0;
		left = NULL;
		right = NULL;
	}

	RootTree(RootTree<T> const& other)
	{
		CopyTree(other);
	}

	RootTree& operator= (RootTree<T> const& other)
	{
		if (this != &other)
		{
			DeleteTree();
			CopyTree(other);
		}
		return *this;
	}

	~RootTree()
	{
		DeleteTree();
	}

	bool Empty() const
	{
		return root == 0;
	}

	T GetRoot() const
	{
		return root;
	}

	void SetRoot(T data)
	{
		root = data;
	}

	RootTree<T>* GetLeft() const
	{
		return left;
	}

	RootTree<T>* GetRight() const
	{
		return right;
	}

	void ChangeFr(RootTree<T> fr)
	{
		queue<RootTree<T> > other;
		other.push(fr);
		rootQueue.pop();
		while (!rootQueue.empty())
		{
			RootTree<T> sth = rootQueue.front();
			other.push(sth);
			rootQueue.pop();
		}
		rootQueue = other;
	}

	void AddChild(T data)
	{
		/* RootTree<T> temp;


		bool completed = false;

		if (root == T())
		{
		temp.root = data;
		temp.left = NULL;
		temp.right = NULL;
		}

		else
		{
		do
		{
		RootTree<T> fr;
		fr = rootQueue.front();


		if (fr.GetLeft() == NULL)
		{
		fr.SetLeft(&temp);
		completed = true;
		ChangeFr(fr);
		}
		else if (fr.right == NULL)
		{
		fr.SetRight(&temp);
		completed = true;
		rootQueue.pop();
		}
		else if (fr.left && fr.right)
		rootQueue.pop();
		} while (completed == false);
		}
		rootQueue.push(temp); */
	}
	void Create()
	{
		T data;
		char c;
		do
		{
			cout << "root y/?n";
			cin >> c;
			cin >> data;
			AddChild(data);
		} while (c == 'y');

	}
	void FillTree(vector<T> Leaves)
	{
		srand(time(0));
		int j = rand() % 10;
		for (int i = 0; i <= j; i++)
			random_shuffle(Leaves.begin(), Leaves.end());

		for (int i = 0; i < Leaves.size(); i++)
			AddChild(Leaves[i]);
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

};
