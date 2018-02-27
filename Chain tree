#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>
#include <cstdlib>
#include <time.h>
#include <fstream>
#include <string>
#include <cstring>

using namespace std;

const int MAX = 10;

template <class T>
class ChainTree
{
private:
	int capacity = MAX;
	int rootPointer;
	T* rootArray = new T[capacity];
	int* leftAddressArray = new int[capacity];
	int* rightAddressArray = new int[capacity];
	vector<string> winners, losers;

	bool Full() const
	{
		return rootArray[capacity - 1] != T();
	}

	void Resize()
	{
		T* tempRoot = rootArray;
		int* tempLeft = leftAddressArray;
		int* tempRight = rightAddressArray;

		rootArray = new T[capacity * 2];
		leftAddressArray = new int[capacity * 2];
		rightAddressArray = new int[capacity * 2];

		for (int i = 0; i < capacity; i++)
		{
			rootArray[i] = tempRoot[i];
			leftAddressArray[i] = tempLeft[i];
			rightAddressArray[i] = tempRight[i];
		}
		for (int i = capacity; i < capacity * 2; i++)
		{
			rootArray[i] = T();
			leftAddressArray[i] = -1;
			rightAddressArray[i] = -1;
		}
		capacity *= 2;
		delete[] tempRoot;
		delete[] tempLeft;
		delete[] tempRight;
	}

	void CopyChainTree(ChainTree const& other)
	{
		rootPointer = other.rootPointer;
		capacity = other.capacity;
		for (int i = 0; i < capacity; i++)
		{
			rootArray[i] = other.rootArray[i];
			leftAddressArray[i] = other.leftAddressArray[i];
			rightAddressArray[i] = other.rightAddressArray[i];
		}
	}

	void DeleteChainTree()
	{
		rootPointer = -1;
		delete[] rootArray;
		delete[] leftAddressArray;
		delete[] rightAddressArray;
	}

public:
	ChainTree()
	{
		rootPointer = -1;
		for (int i = 0; i < capacity; i++)
		{
			rootArray[i] = T();
			leftAddressArray[i] = -1;
			rightAddressArray[i] = -1;
		}
	}

	ChainTree(ChainTree const& other)
	{
		CopyChainTree(other);
	}

	ChainTree& operator = (ChainTree const& other)
	{
		if (this != &other)
		{
			DeleteChainTree();
			CopyChainTree(other);
		}
		return *this;
	}

	~ChainTree()
	{
		DeleteChainTree();
	}

	bool Empty() const
	{
		return rootPointer == -1;
	}

	T RootInfo() const
	{
		return rootArray[rootPointer];
	}

	int RootAddress() const
	{
		return rootPointer;
	}

	void Pr(int rootP) const
	{
		if (rootP != -1)
		{
			Pr(leftAddressArray[rootP]);
			cout << rootArray[rootP] << " ";
			Pr(rightAddressArray[rootP]);
		}
	}

	void Print()
	{
		if (!Empty())
			Pr(rootPointer);
		else
			cout << "Tree is empty!";

		cout << endl;
	}

	void NewElement(T x)
	{
		if (Empty())
			rootPointer = 0;
		if (Full())
			Resize();
		int i = 0;
		while (rootArray[i] != T())
			i++;
		rootArray[i] = x;
		if (i % 2 == 1)
			leftAddressArray[(i - 1) / 2] = i;
		else
			rightAddressArray[(i - 1) / 2] = i;
	}

	void Create()
	{
		T x;
		char c;
		do
		{
			cout << "New Element y/n?"; cin >> c;
			if (c == 'y')
			{
				cout << "Root: "; cin >> x;
				NewElement(x);
			}

		} while (c == 'y');
		rootPointer = 0;
	}

	void FillTree(vector<T> Leaves)
	{
		srand(time(0));
		for (int i = 0; i <= rand() % 10; i++)
			random_shuffle(Leaves.begin(), Leaves.end());

		int numOfCompetitors = Leaves.size();
		int numPlaceholders = 1;

		while (numPlaceholders < numOfCompetitors)
		{
			numPlaceholders *= 2;
		}

		numPlaceholders /= 2;

		string placeholder = "ToBeDetermined 0";
		for (int i = 0; i < numPlaceholders; i++)
			NewElement(placeholder);

		for (int i = 0; i < Leaves.size(); i++)
		{
			NewElement(Leaves[i]);
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

	int DetermineWinner(int const& index1, int const& index2)
	{
		int skill1, skill2;
		int pos1, pos2;
		pos1 = rootArray[index1].find(" ");
		pos1++;
		pos2 = rootArray[index2].find(" ");
		pos2++;
		skill1 = stoi(rootArray[index1].substr(pos1));
		skill2 = stoi(rootArray[index2].substr(pos2));

		srand(time(0));
		int randomBall = rand() % (skill1 + skill2);
		if (randomBall >= 0 && randomBall <= skill1 - 1)
			return index1;
		else
			return index2;
	}

	int NumberOfElements()
	{
		int j = -1;
		while (rootArray[j] != T())
			j++;
		return j;
	}

	void Tournament()
	{
		if (Empty())
		{
			cout << "No participants in tournament!" << endl;
			return;
		}

		int winner;

		int quantity = NumberOfElements() - 1;

		while (quantity >= 1)
		{
			winner = DetermineWinner(quantity, quantity - 1);
			if (winner == quantity)
			{
				winners.push_back(rootArray[quantity]);
				losers.push_back(rootArray[quantity - 1]);

				rootArray[quantity - 1] = rootArray[winner];
				leftAddressArray[quantity - 1] = leftAddressArray[winner];
				rightAddressArray[quantity - 1] = rightAddressArray[winner];
			}
			winners.push_back(rootArray[quantity - 1]);
			losers.push_back(rootArray[quantity]);

			rootArray[quantity] = T();
			leftAddressArray[quantity] = -1;
			rightAddressArray[quantity] = -1;
			quantity = NumberOfElements() - 1;

		}
		cout << "The winner is " << rootArray[0] << endl;
	}

	void AllBattles()
	{
		for (int i = 0; i < winners.size(); i++)
		{
			if (winners[i] != losers[i] && winners[i] != "ToBeDetermined 0" && losers[i] != "ToBeDetermined 0")
			{
				cout << "Match #" << (i + 1) << " " << winners[i] << " vs. " << losers[i] << endl;
				cout << "The winner is " << winners[i] << "!\n\n";
			}
		}
	}

	void FindPath(string name)
	{
		bool found = false;

		for (unsigned int i = 0; i < winners.size(); i++)
		{
			string n = winners[i].substr(0, name.length());
			if (n == name && losers[i] != winners[i])
			{
				cout << winners[i] << " vs. " << losers[i] << endl;
				found = true;
			}

			n = losers[i].substr(0, name.length());
			if (n == name && losers[i] != winners[i])
			{
				cout << losers[i] << " vs. " << winners[i] << endl;
				found = true;
			}
		}

		if (!found)
			cout << "No participant with name " << name << "!" << endl;
	}
};
