#include <iostream> // Активація стандартної бібліотеки.

using namespace std; //  Спрощення написання команд 
                     //  cout, cin, vector, and string 
                     //  без додавання std::.

struct complex   //Оголошення нової структури "complex" + тіло.
{
    int first; //Оголошення поле цілого числа.

    double second; //Оголошення поле дробового числа.

    double power() // Оголошення функції всередині структури
                   // для повернення результату double + тіло.
    {
        return first * second * 10; //Формула калорії.
    } 

}; //Закриття тіли "complex".

int main() //Активації головної функції для обчислень.
{
    system("Color E0"); 

    complex complex; //Створення екземпляра структури.
    cout << endl; cout << endl; cout << endl;
    
    cout << "               |   |       |" << endl;
    cout << "               |   |   o   |" << endl;
    cout << "               |___|   |   |" << endl;
    cout << "               |   |   |   |" << endl;
    cout << "               |   |   |   |" << endl;
    cout << "               |   |   |   0" << endl;

    cout << endl; cout << endl; cout << endl;

    cout << "   first | g |:       ";
    cin >> complex.first; cout << endl; cout << endl; //Зупинка консоля до введене
                                                      //цілого числа complex користувачем.
    cout << "   second | kg |:     "; 
    cin >> complex.second; cout << endl; cout << endl; //Зупинка консоля до введене
                                                       //дробового числа complex користувачем.

    cout << "   !!!!Result!!!!"; cout << endl;
    cout << "           "; cout << complex.power(); //Виведення результату після виконаної
                                                    //операції формули калорії.

    cout << "  kkal"; cout << endl; cout << endl;
	                       
    return 0;
}

