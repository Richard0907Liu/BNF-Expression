#include <iostream>
#include <vector>
#include<string>
#include<stack>
using namespace std;

#define operators_char(_char) ((_char == '+') || (_char == '-') || (_char == '*') || (_char == '/'))


struct node {
	char data;
	struct node *left;
	struct node *right;
};

node* createNode(char value) {
	node *newNode;
	newNode = new node;
	newNode->data = value;
	newNode->left = NULL;
	newNode->right = NULL;

	return newNode;
}

bool compare_pr(char _char, char node_data) {
	if ((_char == '*' || _char == '/') && (node_data == '-' || node_data == '+'))
		return true;
	else if ((_char == '*' || _char == '/') && (node_data == '*' || node_data == '/'))
		return true;
	else if ((_char == '-' || _char == '+') && (node_data == '-' || node_data == '+'))
		return true;
	else
		return false;
}


void buildExpressionTree(stack<node*> &operands, stack<node*> &operators) {
	node *expTree = operators.top();
	operators.pop();
	expTree->left = operands.top();
	operands.pop();
	expTree->right = operands.top();
	operands.pop();
	operands.push(expTree);
}


float evaluate(node* operands) {
	float left, right, counted;
	if (operators_char(operands->data)) {
		left = evaluate(operands->left);
		right = evaluate(operands->right);
		cout << left << ' ' << operands->data << ' ' << right << endl;

		if (operands->data == '+')
			counted = left + right;
		else if (operands->data == '-')
			counted = left - right;
		else if (operands->data == '*')
			counted = left * right;
		else if (operands->data == '/')
			counted = left / right;

		return counted;
	}
	else
		return operands->data - '0';

}

void inorder(node *ptr){
	if (ptr != NULL)
	{
		inorder(ptr->left);
		cout << ptr->data << ", " ;
		inorder(ptr->right);
	}
}
void preorder(node *ptr) {
	if (ptr != NULL)
	{
		cout << ptr->data << ", ";
		preorder(ptr->left);
		preorder(ptr->right);
	}
}

void postorder(node *ptr) {
	if (ptr != NULL)
	{
		
		postorder(ptr->left);
		postorder(ptr->right);
		cout << ptr->data << ", ";
	}
}


int main()
{
	string typing;
	stack<char> input;
	stack<node*> operators;
	stack<node*> operands;
	char processing_char;
	node *temp_node;

	cout << "Please enter an mathematical experssion: "<<endl;
	cin >> typing;

	for (int i = 0; i < typing.length(); i++) {
		input.push(typing[i]);
	}
	
	while (input.size() != 0) {
		processing_char = input.top();
		input.pop();

		if (isdigit(processing_char)) {
			temp_node = createNode(processing_char);
			operands.push(temp_node);
		}
		if (processing_char == ')') {
			temp_node = createNode(processing_char);
			operators.push(temp_node);
		}
		if (operators_char(processing_char)) {
			bool processed = false;
			while (!processed) {
				if (operators.size() == 0) {
					temp_node = createNode(processing_char);
					operators.push(temp_node);
					processed = true;
				}
				else if (operators.top()->data == ')') {
					temp_node = createNode(processing_char);
					operators.push(temp_node);
					processed = true;
				}
				else if(compare_pr(processing_char, operators.top()->data)){
					temp_node = createNode(processing_char);
					operators.push(temp_node);
					processed = true;
				}
				else {
					buildExpressionTree(operands, operators);
				}
			}
		}
		if (processing_char == '(') {
			while (operators.top()->data != ')') {
				buildExpressionTree(operands, operators);
			}
			operators.pop();
		}
	}

	cout << endl << "Processing steps: " << endl;
	while (operators.size() > 0)
		buildExpressionTree(operands, operators);

	cout << "Calculating result: " << evaluate(operands.top()) << endl;

	cout << endl << "Representation of the expression tree" << endl;
	cout << "Inordor printing: " << endl;
	inorder(operands.top());
	cout << endl;

	cout << "Peroder pringing: " << endl;
	preorder(operands.top());
	cout << endl;

	cout << "Postoder pringing: " << endl;
	postorder(operands.top());

	

	return 0;
}
