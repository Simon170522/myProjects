#include <iostream>
#include "math.h"
#define WD 800
#define HT 600
#if defined(linux) || defined(_WIN32)
#include <GL/glut.h>
#else
#include <GLUT/GLUT.h>
#endif

using namespace std;




float t_c = HT*1/20;
class Expression{
public:
    virtual Expression* diff() = 0;             //зануляем ссылку функции в стеке
    virtual void print() = 0;
    virtual float graf(float x) = 0;
};
void reshape(int w, int h)
{
    glViewport(0, 0, w, h);

    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(0, w, 0, h);

    glMatrixMode(GL_MODELVIEW);
    glLoadIdentity();
}

class Number : public Expression{
private:
    int  val;
public:
    Number(double value){
        val = value;
    }
    Expression* diff(){
        return (new Number(0));
    };
    void print(){
        cout << val;
    }
    float graf(float x){
        return val;
    }
};
class Variable : public Expression{
private:
    char x;
public:
    Variable(const char x_in){
        x = x_in;
    }
    Expression* diff(){
        return new Number(1);
    }
    void print(){
        cout << x;
    }
    float graf(float x){
        return x/t_c;
    }
};
class Add : public Expression{
private:
    Expression* left;
    Expression* right;
public:
    Add(Expression* left_in,Expression* right_in){
        left = left_in;
        right = right_in;
    }
    Expression* diff(){
        return new Add(left->diff(),right->diff());
    }
    void print(){
        cout << '(';
        left->print();
        cout << '+';
        right->print();
        cout << ')';
    }
    float graf(float x){
        return left->graf(x) + right->graf(x);
    }
};
class Sub : public Expression{
    Expression* left;
    Expression* right;
public:
    Sub(Expression* left_in,Expression* right_in){
        left = left_in;
        right = right_in;
    }
    Expression* diff(){
        return new Sub(left->diff(),right->diff());
    }
    void print(){
        cout << '(';
        left->print();
        cout << '-';
        right->print();
        cout << ')';
    }
    float graf(float x){
        return left->graf(x) - right->graf(x);
    }
};
class Mul : public Expression{
private:
    Expression* left;
    Expression* right;
public:
    Mul(Expression* left_in,Expression* right_in){
        left = left_in;
        right = right_in;
    }
    Expression* diff(){
        Expression* U = new Mul(left->diff(),right);
        Expression* V = new Mul(left,right->diff());
        return new Add(U,V);
    }
    void print(){
        cout << '(';
        left->print();
        cout << '*';
        right->print();
        cout << ')';
    }
    float graf(float x){
        return left->graf(x) * right->graf(x);
    }
};
class Div : public Expression{
private:
    Expression* left;
    Expression* right;
public:
    Div(Expression* l_in,Expression* r_in) : left(l_in), right(r_in){}
    Expression* diff(){
        Expression* mnog = new Sub(new Mul(left->diff(),right),new Mul(left,right->diff()));
        Expression* chastnoe = new Mul(right,right);
        return new Div(mnog,chastnoe);
    }
    void print(){
        cout << "(";
        left->print();
        cout << "/";
        right->print();
        cout << ")";
    }
    float graf(float x){
        return left->graf(x) / right->graf(x);
    }
};
class Cos : public Expression{
private:
    Expression* arg;
public:
    Cos(Expression* a_in) : arg(a_in){};
    Expression* diff();
    void print(){
        cout << "(cos ";
        arg->print();
        cout << ")";
    }
    float graf(float x){
        return cos(arg->graf(x));
    }
};
class Sin : public Expression{
private:
    Expression* arg;
public:
    Sin(Expression* a_in) : arg(a_in){};
    Expression* diff(){
        return new Mul(arg->diff(),new Cos(arg));
    }
    void print(){
        cout << "(sin ";
        arg->print();
        cout << ")";
    }
    float graf(float x){
        return sin(arg->graf(x));
    }
};

Expression *Cos::diff() {
    return new Mul(arg->diff(),new Sub(new Number(0),new Sin(arg)));
}

void display(){                     // функция отрисовки
    glClear(GL_COLOR_BUFFER_BIT);
    Expression* suma = new Sin(new Variable('x'));
    glBegin(GL_LINES);
    glColor3f(1.0, 1.0, 1.0);
    glVertex2i(0, HT/2);
    glVertex2i(WD, HT/2);
    glVertex2i(WD/2, 0);
    glVertex2i(WD/2, HT);
    glVertex2i(WD/2 + t_c, HT/2 + 5);
    glVertex2i(WD/2 + t_c, HT/2 - 5);
    glVertex2i(WD/2 + 5, HT/2 + t_c);
    glVertex2i(WD/2 - 5, HT/2 + t_c);
    glEnd();
    glBegin(GL_LINE_STRIP);
    glColor3f(1.0, 1.0, 0);
    int x,y;
    Expression* e = suma;
    e->print();
    for(int x1 = WD*(-1)/2; x1 < WD/2;x1 += 10){
        x = x1 + WD/2;
        y = HT/2 + t_c * e->graf(x1);
        glVertex2i(x,y);
    }
    glEnd();

    glutSwapBuffers();
}

int main (int argc, char * argv[])
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE|GLUT_RGB); /*Включаем двойную буферизацию и четырехкомпонентный цвет*/

    glutInitWindowSize(WD, HT);
    glutCreateWindow("GL Graphick FUNCTION");

    glutReshapeFunc(reshape);
    glutDisplayFunc(display);

    glutMainLoop();

    return 0;
}
