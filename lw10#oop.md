#include <iostream>
#include <cmath>
#include <vector>
#include <stdexcept>
#include <memory>

using namespace std;

struct point {
    double x, y;
    
    double distance(const point& other) const {
        return sqrt((x - other.x) * (x - other.x) + (y - other.y) * (y - other.y));
    }
};

class Triangle {
public:
    virtual ~Triangle() = default;

    virtual double gsA() const = 0;
    virtual double gsB() const = 0;
    virtual double gsC() const = 0;

    virtual double p() const {
        return gsA() + gsB() + gsC();
    }

    virtual double area() const {
        double a = gsA();
        double b = gsB();
        double c = gsC();
        double s = p() / 2.0;
        return sqrt(s * (s - a) * (s - b) * (s - c));
    }

    double median(char sideName) const {
        double a = gsA();
        double b = gsB();
        double c = gsC();

        if (sideName == 'a' || sideName == 'A') {
            return 0.5 * sqrt(2.0 * b * b + 2.0 * c * c - a * a);
        } else if (sideName == 'b' || sideName == 'B') {
            return 0.5 * sqrt(2.0 * a * a + 2.0 * c * c - b * b);
        } else if (sideName == 'c' || sideName == 'C') {
            return 0.5 * sqrt(2.0 * a * a + 2.0 * b * b - c * c);
        }
        throw invalid_argument("Невідома назва сторони для медіани!");
    }

    double paralel(char sideName) const {
        if (sideName == 'a' || sideName == 'A') return gsA() / 2.0;
        if (sideName == 'b' || sideName == 'B') return gsB() / 2.0;
        if (sideName == 'c' || sideName == 'C') return gsC() / 2.0;
        throw invalid_argument("Невідома назва сторони для середньої лінії!");
    }

    virtual void inform() const = 0;
};

class vertice : public Triangle {
private:
    point p1, p2, p3;
    double a, b, c;

    void validate() {
        a = p1.distance(p2);
        b = p2.distance(p3);
        c = p3.distance(p1);
        if (a + b <= c || a + c <= b || b + c <= a) {
            throw invalid_argument("Трикутник не існує.");
        }
    }

public:
    vertice(point v1, point v2, point v3) : p1(v1), p2(v2), p3(v3) {
        validate();
    }

    double gsA() const override { return a; }
    double gsB() const override { return b; }
    double gsC() const override { return c; }

    void inform() const override {
        cout << "Вершини: (" << p1.x << "," << p1.y << "), (" 
             << p2.x << "," << p2.y << "), (" << p3.x << "," << p3.y << ")\n"
             << "Сторони: a=" << a << ", b=" << b << ", c=" << c << "\n";
    }
};

class TriangleBySides : public Triangle {
protected:
    double a, b, c;

    void validate(double sideA, double sideB, double sideC) {
        if (sideA <= 0 || sideB <= 0 || sideC <= 0) {
            throw invalid_argument("Некоректні сторони.");
        }
        if (sideA + sideB <= sideC || sideA + sideC <= sideB || sideB + sideC <= sideA) {
            throw invalid_argument("Трикутник не існує.");
        }
    }

public:
    TriangleBySides(double sideA, double sideB, double sideC) {
        validate(sideA, sideB, sideC);
        a = sideA;
        b = sideB;
        c = sideC;
    }

    double gsA() const override { return a; }
    double gsB() const override { return b; }
    double gsC() const override { return c; }

    static TriangleBySides fromMedians(double ma, double mb, double mc) {
        if (ma <= 0 || mb <= 0 || mc <= 0) {
            throw invalid_argument("Некоректні медіани.");
        }
        if (ma + mb <= mc || ma + mc <= mb || mb + mc <= ma) {
            throw invalid_argument("Трикутник не існує.");
        }

        double a = (2.0 / 3.0) * sqrt(2.0 * mb * mb + 2.0 * mc * mc - ma * ma);
        double b = (2.0 / 3.0) * sqrt(2.0 * ma * ma + 2.0 * mc * mc - mb * mb);
        double c = (2.0 / 3.0) * sqrt(2.0 * ma * ma + 2.0 * mb * mb - mc * mc);

        return TriangleBySides(a, b, c);
    }

    void inform() const override {
        cout << "Сторони: a=" << a << ", b=" << b << ", c=" << c << "\n";
    }
};

class righttriangle : public TriangleBySides {
private:
    void validateRight() {
        if (abs((a * a + b * b) - (c * c)) > 1e-5) {
            throw invalid_argument("Трикутник не є прямокутним.");
        }
    }

public:
    righttriangle(double katet1, double katet2, double hypotenuse) 
        : TriangleBySides(katet1, katet2, hypotenuse) {
        validateRight();
    }

    double inr() const {
        return (a + b - c) / 2.0;
    }

    double cir() const {
        return c / 2.0;
    }

    void inform() const override {
        cout << "Катети: " << a << " і " << b << ", Гіпотенуза: " << c << "\n";
    }
};

int main() {
    try {
        point v1{0, 0}, v2{4, 0}, v3{0, 3};
        vertice tv(v1, v2, v3);
        tv.inform();
        cout << "Периметр: " << tv.p() << "\n";
        cout << "Площа: " << tv.area() << "\n";
        cout << "Медіана: " << tv.median('c') << "\n";
        cout << "Паралельна середня лінія: " << tv.paralel('c') << "\n\n";

        auto tMed = TriangleBySides::fromMedians(6.0, 6.0, 4.0);
        tMed.inform();
        cout << "Площа за медіанами: " << tMed.area() << "\n\n";

        righttriangle rt(3.0, 4.0, 5.0);
        rt.inform();
        cout << "Вписаний радіус: " << rt.inr() << "\n";
        cout << "Описаний радіус: " << rt.cir() << "\n";

    } catch (const exception& e) {
        cerr << "Помилка: " << e.what() << "\n";
    }

    return 0;
}
