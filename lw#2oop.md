
#include <iostream>
#include <cmath>

using namespace std;

class Angle {
    int minut; 
    static constexpr int minDegree = 60;
    static constexpr int degrees = 360;
    static constexpr int circle = degrees * minDegree;
    static constexpr double PI = 3.14;

    void normalize() {
        minut %= circle;
        if (minut < 0) minut += circle;
    }

public:
    Angle(int deg = 0, int min = 0) {
        int sign = (deg < 0 || (deg == 0 && min < 0)) ? -1 : 1;
        minut = sign * (std::abs(deg) * minDegree + std::abs(min));
        normalize();
    }

    
    static Angle fromMinutes(int minutes) {
        Angle a;
        a.minut = minutes;
        a.normalize();
        return a;
    }

    void getDegMin(int& deg, int& min) const {
        deg = minut / minDegree;
        min = minut % minDegree;
    }

    double toRadians() const {
        double degrees = minut / static_cast<double>(minDegree);
        return degrees * PI / 180.0;
    }

    void increase(const Angle& other) { minut += other.minut; normalize(); }
    void decrease(const Angle& other) { minut -= other.minut; normalize(); }

    double sin() const { return std::sin(toRadians()); }

    Angle operator+(const Angle& o) const { return fromMinutes(minut + o.minut); }
    Angle operator-(const Angle& o) const { return fromMinutes(minut - o.minut); }

    bool operator==(const Angle& o) const { return minut == o.minut; }
    bool operator!=(const Angle& o) const { return !(*this == o); }
    bool operator<(const Angle& o) const { return minut < o.minut; }
    bool operator<=(const Angle& o) const { return minut <= o.minut; }
    bool operator>(const Angle& o) const { return minut > o.minut; }
    bool operator>=(const Angle& o) const { return minut >= o.minut; }

    friend std::ostream& operator<<(std::ostream& os, const Angle& a) {
        int d, m;
        a.getDegMin(d, m);
        os << d << "deg" << m << "'";
        return os;
    }
};

int main() 
{
    cout << "  _____     _____     _____ " << endl;
    cout << "  _____     _____   ||_____|| " << endl;
    cout << "||     || ||     || ||_____|| " << endl;
    cout << "||     || ||     || ||_____|| " << endl;
    cout << "||     || ||     || ||" << endl;
    cout << "  _____     _____   ||" << endl;

    cout << "          " << endl;

    cout << "after input degree make press space or enter for input radian " << endl;

    int deg, min;

    cout << "angle A -> : ";
    if (!(cin >> deg >> min)) return 0;
    Angle a(deg, min);

    cout << "angle B -> : ";
    if (!(cin >> deg >> min)) return 0;
    Angle b(deg, min);

    cout << "a = " << a << endl;
    cout << "b = " << b << endl;

    Angle c = a + b;
    cout << "a + b = " << c << endl;

    cout << "result -> ";
    if (!(cin >> deg >> min)) return 0;
    c.decrease(Angle(deg, min));
    cout << "degree: " << c << endl;

    cout << "sin = " << a.sin() << endl;
    cout << "radian = " << a.toRadians() << endl;

    return 0;
}
