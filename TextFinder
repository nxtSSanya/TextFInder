#pragma GCC optimize("O3")
#pragma comment(linker, "/STACK:36777216")
#define _CRT_SECURE_NO_WARNINGS
#define all(x) begin(x), end(x)
#include <iostream>
#include <fstream>
#include <functional>
#include <cstdio>
#include <climits>
#include <cmath>
#include <map>
#include <set>
#include <thread>
#include <chrono>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <string>
#include <cstring>
#include <stack>
#include <queue>
#include <vector>
#include <cassert>
//#include <bitset>
#define ll long long
#define ull unsigned long long
#define endl "\n"
#define rt(x) return(x)
using namespace std;
map <int, long long> inversions;
vector<int> Si;
const int INF = 1 << 30;
const ll LINF = 1ll << 62;

const int MAX_RESULT_DOCUMENT_COUNT = 5;

string ReadLine() {
	string s;
	getline(cin, s);
	return s;
}

int ReadLineWithNumber() {
	int result;
	cin >> result;
	ReadLine();
	return result;
}

vector<string> SplitIntoWords(const string& text) {
	vector<string> words;
	string word;
	for (const char c : text) {
		if (c == ' ') {
			words.push_back(word);
			word = "";
		}
		else {
			word += c;
		}
	}
	words.push_back(word);

	return words;
}

struct Document {
	int id;
	double relevance;
};

class SearchServer {
public:
	void SetStopWords(const string& text) {
		for (const string& word : SplitIntoWords(text)) {
			stop_words_.insert(word);
		}
	}

	void AddDocument(int document_id, const string& document) {
		vector < string > document_words = SplitIntoWordsNoStop(document);
		for (const string& word : document_words) {
			word_to_documents_freqs_[word][document_id] += 1.0 / document_words.size();
			// cout<<document_id<<" "<<document_words.size()<<" "<<(1.0/document_words.size())<<endl;
		}
		++document_count;
	}

	vector<Document> FindTopDocuments(const string& query) const {
		auto matched_documents = FindAllDocuments(query);

		sort(
			matched_documents.begin(),
			matched_documents.end(),
			[](const Document& lhs, const Document& rhs) {
			return lhs.relevance > rhs.relevance;
		}
		);
		if (matched_documents.size() > MAX_RESULT_DOCUMENT_COUNT) {
			matched_documents.resize(MAX_RESULT_DOCUMENT_COUNT);
		}
		return matched_documents;
	}

private:
	map<string, map< int, double >> word_to_documents_freqs_;
	set<string> stop_words_;
	int document_count = 0;
	vector<string> SplitIntoWordsNoStop(const string& text) const {
		vector<string> words;
		for (const string& word : SplitIntoWords(text)) {
			if (stop_words_.count(word) == 0) {
				words.push_back(word);
			}
		}
		return words;
	}

	vector<Document> FindAllDocuments(const string& query) const {
		const vector <string> query_words = SplitIntoWordsNoStop(query);
		map <int, double > document_to_relevance;
		set < string > minus_words;
		for (const string& word : query_words) {
			if (word[0] == '-') {
				string minus_word = word;
				minus_word.erase(minus_word.begin());
				minus_words.insert(minus_word);
				continue;
			}
			if (word_to_documents_freqs_.count(word) == 0) {
				continue;
			}
			double word_IDF = log((double)document_count / word_to_documents_freqs_.at(word).size());
			//cout<<word_IDF<<endl;
			for (auto& document_id : word_to_documents_freqs_.at(word)) {
				//cout<<document_id.first<<" "<< document_id.second<<endl;
				document_to_relevance[document_id.first] += (word_IDF * document_id.second);
			}
		}

		set< int > minus_documents;
		for (const string& word : minus_words) {
			if (word_to_documents_freqs_.count(word) != 0)
				for (auto& i : word_to_documents_freqs_.at(word))
					minus_documents.insert(i.first);
		}

		vector<Document> matched_documents;
		for (auto q : document_to_relevance) {
			if (minus_documents.count(q.first) == 0) {
				matched_documents.push_back({ q.first, q.second });
			}
		}
		return matched_documents;
	}
};

SearchServer CreateSearchServer() {
	SearchServer search_server;
	search_server.SetStopWords(ReadLine());

	const int document_count = ReadLineWithNumber();
	for (int document_id = 0; document_id < document_count; ++document_id) {
		search_server.AddDocument(document_id, ReadLine());
	}

	return search_server;
}


int main() {
	const SearchServer search_server = CreateSearchServer();

	const string query = ReadLine();
	for (auto &out : search_server.FindTopDocuments(query)) {
		cout << "document_id = " << out.id << ", relevance = " << out.relevance << endl;
	}
}
