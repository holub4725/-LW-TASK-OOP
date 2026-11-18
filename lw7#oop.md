#include <iostream>
#include <cmath>
#include <stdexcept>
#include <string>

using namespace std;

class ee {};

class ce {
private:
    string message;
    double b1;
    double q;
    int n;

public:
    ce(string msg, double first, double ratio, int count)
        : message(msg), b1(first), q(ratio), n(count) {}

    void what() const {
        cout << "помилка (ce): " << message 
                  << " [b1 = " << b1 << ", q = " << q << ", n = " << n << "]\n";
    }
};

class de : public runtime_error {
private:
    double q;
    int n;
public:
    de(const string& msg, double ratio, int count)
        : runtime_error(msg), q(ratio), n(count) {}

    double getQ() const { return q; }
    int getN() const { return n; }
};

double ca1(double b1, double q, int n) {
    if (n <= 0) {
        throw invalid_argument("Кількість членів прогресії має бути більшою за 0.");
    }
    if (q == 1.0) {
        return b1 * n;
    }
    return b1 * (1.0 - pow(q, n)) / (1.0 - q);
}

double ca2(double b1, double q, int n) noexcept(false) {
    if (n <= 0) {
        throw invalid_argument("Кількість членів не може бути від'ємною або нульовою.");
    }
    return b1 * (1.0 - pow(q, n)) / (1.0 - q);
}

double ca3(double b1, double q, int n) {
    if (n <= 0) {
        throw out_of_range("Помилка: n повинно бути додатним числом.");
    }
    return b1 * (1.0 - pow(q, n)) / (1.0 - q);
}

double ca4(double b1, double q, int n) {
    if (n <= 0) {
        throw ce("Некоректні параметри для геометричної прогресії", b1, q, n);
    }
    return b1 * (1.0 - pow(q, n)) / (1.0 - q);
}

int main() {
    cout << "без специфікації виключень\n";
    try {
        cout << "сума: " << ca1(2.0, 2.0, 3) << "\n";
        ca1(2.0, 2.0, -1); 
    } catch (const exception& e) {
        cout << "перехоплено виключення: " << e.what() << "\n\n";
    }

    cout << "з noexcept\n";
    try {
        cout << "сума: " << ca2(1.0, 3.0, 4) << "\n";
        ca2(1.0, 3.0, 0);
    } catch (const exception& e) {
        cout << "перехоплено виключення: " << e.what() << "\n\n";
    }

    cout << "зі стандартним виключенням\n";
    try {
        ca3(5.0, 2.0, -3);
    } catch (const out_of_range& e) {
        cout << "перехоплено out_of_range: " << e.what() << "\n\n";
    }

    cout << "з власним виключенням\n";
    try {
        ca4(1.0, 2.0, 0);
    } catch (const ce& e) {
        e.what();
    }

    return 0;
}
