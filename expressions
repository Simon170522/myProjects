#include <iostream>
using namespace std;

class Expression{
public:
    virtual Expression* diff() = 0;             //зануляем ссылку функции в стеке
    virtual void print() = 0;
    virtual void pPrint() = 0;
};
FILE *gp = popen("gnuplot -persist","w");
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
    void pPrint(){
        fprintf(gp,"(%d)",val);
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
    void pPrint(){
        fprintf(gp,"(%c)",x);
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
    void pPrint(){
        fprintf(gp,"(");
        left->pPrint();
        fprintf(gp," + ");
        right->pPrint();
        fprintf(gp,")");
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
    void pPrint(){
        fprintf(gp,"(");
        left->pPrint();
        fprintf(gp," - ");
        right->pPrint();
        fprintf(gp,")");
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
    void pPrint(){
        fprintf(gp,"(");
        left->pPrint();
        fprintf(gp," * ");
        right->pPrint();
        fprintf(gp,")");
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
    void pPrint(){
        fprintf(gp,"(");
        left->pPrint();
        fprintf(gp," / ");
        right->pPrint();
        fprintf(gp,")");
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
    void pPrint(){
        fprintf(gp,"(cos");
        arg->pPrint();
        fprintf(gp,")");
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
    void pPrint(){
        fprintf(gp,"(sin");
        arg->pPrint();
        fprintf(gp,")");
    }
};

Expression *Cos::diff() {
    return new Mul(arg->diff(),new Sub(new Number(0),new Sin(arg)));
}
int main() {
    float a,b;
    fprintf(gp,"set grid \n");
    cout << "\n input x range [a,b] \n";
    cin >> a >> b;
    fprintf(gp,"set xrange [ %f : %f ] \n",a,b);
    cout << "\n input y range [a,b] \n";
    cin >> a >> b;
    fprintf(gp,"set yrange [ %f : %f ] \n",a,b);
    fprintf(gp,"plot ");
    Expression* sum1 = new Mul(new Sin(new Variable('x')),new Variable('x'));
    sum1->diff()->diff()->pPrint();
    fprintf(gp,"\n");
    pclose(gp);
    return 0;
}