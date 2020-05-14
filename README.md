#include <iostream>

#include <map>

using namespace std;


struct Point {

	int x, y;

	Point(int a, int b): x(a), y(b) {}

};


//Global operator

bool operator<(const Point& a, const Point& b) {

	return (a.x < b.x || (a.x == b.x && a.y < b.y));

}


class Matrix {

private:

	map<Point, int> cells;

	int maxX, maxY;


public:

	Matrix(): maxX(0), maxY(0) {}

	void set(int x, int y, int value);

	int get(int x, int y);

	bool sum(Matrix& other);

	void product(int coefficient);

	int getRowCount() {return maxY+1;}

	int getColCount() {return maxX+1;}

	void print();

};


void Matrix::set(int x, int y, int value) {

	Point t(y, x);


	//Check whether a value exists

	if (value != 0) {

		cells[t] = value;

		maxY = max(y, maxY);

		maxX = max(x, maxX);

	}

	else {

		if (cells.count(t)) cells.erase(t);

	}

}


int Matrix::get(int x, int y) {

	Point t(y, x);

	//Return the value at x, y

	return cells[t];

}


void Matrix::product(int coefficient) {

	for (pair<Point, int> el : cells) {

		//Multiply each non-zero cell by the coefficient

		cells[el.first] *= coefficient;

	}

}


bool Matrix::sum(Matrix& other) {

	//Check if they have the same dimension

	return getColCount() == other.getColCount() && getRowCount() == other.getRowCount();

}


void Matrix::print() {

	for (int row = 0; row <= maxY; row++) {

		for (int col = 0; col <= maxX; col++) {

			Point t(row, col);


			if (cells.count(t)) {

				cout << cells[t] << " ";

			} else cout << 0 << " ";

		}

		cout << "\n";	

	}

	cout << "\n";

}


int main() {

  //Sample Code

	Matrix matrix;

	matrix.set(4,3,5);

	matrix.set(3,4,6);

	matrix.print();

	matrix.product(2);

	matrix.print();

}

