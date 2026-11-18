#include <iostream>

using namespace std;

class Base 
{
public:
    virtual void myname() { 
        cout << "This is class Base" << endl; 
    }
    virtual ~Base() {} 
};

class DerA : virtual public Base 
{
public:
    void myname() override { 
        cout << "This is class DerA" << endl; 
    }
};

class DerB : virtual public Base 
{
public:
    void myname() override { 
        cout << "This is class DerB" << endl; 
    }
};

class DerAB : public DerA, public DerB 
{
public:
    void myname() override { 
        cout << "This is class DerAB" << endl; 
    }
};

int main() {
    Base objBase;
    DerA objDerA;
    DerB objDerB;
    DerAB objDerAB;

    objBase.myname();
    objDerA.myname();
    objDerB.myname();
    objDerAB.myname();

    Base* ptrs[4];

    ptrs[0] = new Base();
    ptrs[1] = new DerA();
    ptrs[2] = new DerB();
    ptrs[3] = new DerAB();

    for (int i = 0; i < 4; i++) {
        ptrs[i]->myname();
    }

    for (int i = 0; i < 4; i++) {
        delete ptrs[i];
    }

    return 0;
}
