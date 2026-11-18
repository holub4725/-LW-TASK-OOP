#include <iostream>
#include <cmath>
#include <stdexcept>

using namespace std;

#ifndef M_PI
#define M_PI = 3.14
#endif

class t3x {
protected:
    double a, b, c;

public:
    t3x(double a = 0.0, double b = 0.0, double c = 0.0) : a(a), b(b), c(c) {}
    
    virtual ~t3x() = default;

    void setA(double val) { a = val; }
    void setB(double val) { b = val; }
    void setC(double val) { c = val; }
    
    void setAll(double a_val, double b_val, double c_val) {
        a = a_val;
        b = b_val;
        c = c_val;
    }

    double getA() const { return a; }
    double getB() const { return b; }
    double getC() const { return c; }

    double sum() const {
        return a + b + c;
    }

    virtual void inform() const {
        cout << "t3x: (" << a << ", " << b << ", " << c 
             << "), сума = " << sum() << endl;
    }
};

class t3xangle : public t3x {
private:
    bool isValid(double s1, double s2, double s3) const {
        return (s1 > 0 && s2 > 0 && s3 > 0) &&
               (s1 + s2 > s3) && (s1 + s3 > s2) && (s2 + s3 > s1);
    }

public:
    t3xangle(double sideA = 1.0, double sideB = 1.0, double sideC = 1.0) : t3x(sideA, sideB, sideC) {
        if (!isValid(sideA, sideB, sideC)) {
            throw invalid_argument("трикутник з такими сторонами не існує!");
        }
    }

    void setSides(double sideA, double sideB, double sideC) {
        if (!isValid(sideA, sideB, sideC)) {
            throw invalid_argument("некоректні сторони трикутника!");
        }
        setAll(sideA, sideB, sideC);
    }

    double getArea() const {
        double p = sum() / 2.0; 
        return sqrt(p * (p - a) * (p - b) * (p - c));
    }

    void getAngles(double& angleA, double& angleB, double& angleC) const {
        double cosA = (b * b + c * c - a * a) / (2.0 * b * c);
        double cosB = (a * a + c * c - b * b) / (2.0 * a * c);
        double cosC = (a * a + b * b - c * c) / (2.0 * a * b);

        angleA = acos(cosA) * 180.0 / M_PI;
        angleB = acos(cosB) * 180.0 / M_PI;
        angleC = acos(cosC) * 180.0 / M_PI;
    }

    void inform() const override {
        double alpha, beta, gamma;
        getAngles(alpha, beta, gamma);
        cout << "t3xangle зі сторонами (" << a << ", " << b << ", " << c << "):\n"
             << "  периметр (сума) = " << sum() << "\n"
             << "  площа = " << getArea() << "\n"
             << "  кути (градуси): a = " << alpha << ", b = " << beta << ", c = " << gamma << endl;
    }
};

t3x p3x(t3x obj) {
    cout << "суму чисел: " << obj.sum() << endl;
    obj.setAll(obj.getA() + 1, obj.getB() + 1, obj.getC() + 1);
    return obj;
}

void ds(const t3x& baseRef) {
    baseRef.inform();
}

int main() {
    setlocale(LC_ALL, "uk_UA.UTF-8");

    try {
        t3x t1(2.5, 4.0, 3.5);
        t1.inform();

        t3xangle tr1(3.0, 4.0, 5.0); 
        tr1.inform();

        cout << "\np3x" << endl;
        t3x res1 = p3x(t1);
        cout << "t3x: ";
        res1.inform();

        t3x res2 = p3x(tr1); 
        cout << "t3xangle: ";
        res2.inform();

        
        ds(t1);  
        ds(tr1); 

    } catch (const exception& e) {
        cerr << e.what() << endl;
    }

    return 0;
}
