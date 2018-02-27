#include <iostream>
#include <queue>
#include <stdlib.h>
#include <time.h>
#include <vector>
#include <random>

using namespace std;

const int INITIAL = 50;

template <class T>
class CmpBinTree
{
	T *nodes;
	int nodesCount, tSize;
	vector<string> fights;

	void DeleteTree();
	void CopyTree(CmpBinTree<T> const&);
	void resize();

public:
	CmpBinTree();
	CmpBinTree(CmpBinTree<T> const&);
	CmpBinTree& operator=(CmpBinTree<T> const&);
	~CmpBinTree();

	T GetRoot() const;
	void CreateTree();
	void AddNode(T const);
	bool empty() const;
	void print() const;
	int size() const;

	void filltree(vector<T>);
	vector<string> GetParticipants();
	void Tournament();
	void TournamentIterate(int);
	int DetermineWinner(int, int);
	void allFights();
	void findpath(string);
};

template <class T>
CmpBinTree<T>::CmpBinTree()
{
	tSize = INITIAL;
	nodes = new T[tSize];
	nodesCount = 0;

	for (int i = 0; i < tSize; i++)
	{
		nodes[i] = T();
	}
}

template <class T>
CmpBinTree<T>::CmpBinTree(CmpBinTree<T> const& tree)
{
	CopyTree(tree);
}

template <class T>
CmpBinTree<T>& CmpBinTree<T>::operator =(CmpBinTree<T> const& tree)
{
	if (this != &tree)
	{
		DeleteTree();
		CopyTree(tree);
	}

	return *this;
}

template <class T>
CmpBinTree<T>::~CmpBinTree()
{
	DeleteTree();
}

template <class T>
void CmpBinTree<T>::DeleteTree()
{
	delete[] nodes;
	nodesCount = 0;
}

template <class T>
void CmpBinTree<T>::CopyTree(CmpBinTree<T> const& tree)
{
	nodesCount = tree.nodesCount;
	delete[] nodes;
	nodes = new T[nodesCount];

	for (int i = 0; i < nodesCount; i++)
		nodes[i] = tree.nodes[i];
}

template <class T>
void CmpBinTree<T>::resize()
{
	T *h = new T[tSize];

	for (int i = 0; i < tSize; i++)
		h[i] = nodes[i];

	nodes = new T[tSize * 2];

	for (int i = 0; i < tSize; i++)
		nodes[i] = h[i];

	tSize *= 2;
}

template <class T>
T CmpBinTree<T>::GetRoot() const
{
	return nodes[0];
}

template <class T>
int CmpBinTree<T>::size() const
{
	return nodesCount;
}

template <class T>
bool CmpBinTree<T>::empty() const
{
	return nodesCount == 0;
}

template <class T>
void CmpBinTree<T>::CreateTree()
{
	queue<T> q;
	char c;
	T x;
	int index = 1;

	cout << "root: ";
	cin >> x;

	nodes[0] = x;
	q.push(x);
	nodesCount++;

	while (true)
	{
		cout << "left node of " << q.front() << " y/n? ";
		cin >> c;

		if (c == 'y')
		{
			cout << "node: ";
			cin >> x;
			nodes[index++] = x;
			q.push(x);
			nodesCount++;
			if (index == tSize - 1)
				resize();
		}
		else
			break;

		cout << "right node of " << q.front() << " y/n? ";
		cin >> c;

		if (c == 'y')
		{
			cout << "node: ";
			cin >> x;
			nodes[index++] = x;
			q.push(x);
			nodesCount++;
			q.pop();
			if (index == tSize - 1)
				resize();
		}
		else
			break;
	}
}

template <class T>
void CmpBinTree<T>::AddNode(T const x)
{
	if (nodesCount == tSize - 1)
		resize();

	nodes[nodesCount++] = x;
}

template <class T>
void CmpBinTree<T>::print() const
{
	if (empty())
	{
		cout << "The tree is empty!" << endl;
		return;
	}

	queue<int> q;
	int index = -1;

	q.push(0);

	cout << nodes[0] << " ";

	while (!q.empty())
	{
		index++;
		if (2 * index + 1 < nodesCount)
		{
			cout << nodes[2 * index + 1] << " ";
			q.push(2 * index + 1);
		}

		if (2 * index + 2 < nodesCount)
		{
			cout << nodes[2 * index + 2] << " ";
			q.push(2 * index + 2);
		}

		q.pop();
	}

	cout << endl;
}

template <typename T>
void CmpBinTree<T>::filltree(vector<T> participants)
{
	int layers = 0;
	unsigned int a = 1;

	while (a < participants.size())
	{
		a *= 2;
		layers++;
	}

	a = 1;
	string emptyElement = T();

	for (int i = 0; i < layers; i++)
	{
		for (unsigned int j = 0; j < a; j++)
		{
			AddNode(emptyElement);
		}

		a *= 2;
	}

	srand(time(0));

	for (int i = 0; i <= rand() % 10; i++)
		random_shuffle(participants.begin(), participants.end());

	for (unsigned int i = 0; i < participants.size(); i++)
		AddNode(participants[i]);
}

template <typename T>
vector<string> CmpBinTree<T>::GetParticipants()
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

template <typename T>
void CmpBinTree<T>::Tournament()
{
	if (empty())
	{
		cout << "No participants in tournament!" << endl;
		return;
	}

	if (nodesCount == 1)
	{
		cout << "The winner is " << nodes[0] << endl;
		return;
	}

	while (nodes[0] == T())
		TournamentIterate(0);
}

template <typename T>
void CmpBinTree<T>::TournamentIterate(int index)
{
	int leftIndex = 2 * index + 1, rightIndex = 2 * index + 2;

	if (nodes[leftIndex] != T())
	{
		if (nodes[rightIndex] != T() && rightIndex < nodesCount)
		{
			int winner;
			winner = DetermineWinner(leftIndex, rightIndex);
			if (winner == leftIndex)
				nodes[index] = nodes[leftIndex];
			else
				nodes[index] = nodes[rightIndex];
		}
		else
			nodes[index] = nodes[leftIndex];

		return;
	}

	if(leftIndex < nodesCount)
		TournamentIterate(leftIndex);
	if(rightIndex < nodesCount)
		TournamentIterate(rightIndex);
}

template <typename T>
int CmpBinTree<T>::DetermineWinner(int part1, int part2)
{
	int skill1, skill2, pos1, pos2;
	string name1, name2;

	pos1 = nodes[part1].find(" ") + 1;
	pos2 = nodes[part2].find(" ") + 1;
	skill1 = stoi(nodes[part1].substr(pos1));
	skill2 = stoi(nodes[part2].substr(pos2));

	name1 = nodes[part1].substr(0, pos1 - 1);
	name2 = nodes[part2].substr(0, pos2 - 1);

	fights.push_back(name1 + " VS " + name2);

	srand(time(0));

	int randomBall = rand() % (skill1 + skill2);

	if (randomBall >= 0 && randomBall <= skill1 - 1)
		return part1;
	else
		return part2;
}

template <typename T>
void CmpBinTree<T>::findpath(string name)
{
	if (fights.empty())
	{
		cout << "No participants in tournament!" << endl;
		return;
	}

	bool found = false;

	for (unsigned int i = 0; i < fights.size(); i++)
	{
		string f = fights[i];

		int pos = f.find(" ");
		string n1 = f.substr(0, pos);
		string n2 = f.substr(pos + 4);

		if (n1 == name || n2 == name)
		{
			cout << fights[i] << endl;
			found = true;
		}
	}

	if (!found)
	{
		cout << "No participant with name " << name << "!" << endl;
	}
}

template <typename T>
void CmpBinTree<T>::allFights()
{
	for (unsigned int i = 0; i < fights.size(); i++)
	{
		cout << fights[i] << endl;
	}

	cout << "The winner is " << nodes[0] << endl;
}
