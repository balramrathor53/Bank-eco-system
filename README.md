# Bank-eco-system
“A feature-rich console based Bank Management System built in C++ using OOP and file handling. It supports secure account creation, PIN-based login, deposits, withdrawals, balance checking, transaction history, account search, update and deletion. Data is stored persistently in files, making it a practical and real-world C++ application.”
#include <iostream>
#include <fstream>
#include <limits>
#include <ctime>
using namespace std;
class Bank{
        private:
    long long Accountno;
    long pin;
    string name,accounttype;
    double balance;
    public:
    string currentDateTime(){
        time_t now=time(0);
        return ctime(&now);
    }
    long cpin;
    long long Adharno;
    void Accountcreat(){
        cout<<"---------------Account Create--------------------\n";
        ofstream o("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt",ios::app);
        cout<<"ENTER NAME: ";
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        getline(cin,name);
        cout<<"ENTER ACCOUNT TYPE:(Saving/Current)";
        getline(cin,accounttype);
        cout<<"ENTER BLANCE :";
        cin>>balance;
        if(accounttype=="Saving"&&balance<1000){
            cout<<"MIMIMUM BLANCE FOR SAVING ACCOUNT 1000"<<endl;
            return;
        }
        cout<<"ENTER ADHAR NUMBER : ";
        cin>>Adharno;
        Accountno=Adharno-9875;
        cout<<"CREATE PIN :";
        cin>>pin;
        cout<<"CONFIRM PIN: ";
        cin>>cpin;
        if(cpin==pin){
            cout<<"Account created Successfully:"<<endl;
              cout<<"YOUR ACCOUNT NUMBER ="<<Accountno<<endl;
        }
        else{
            cout<<"Please Enter confirm pin:"<<endl;
        }
      
         o<<name<<"|"<<Accountno<<"|"<<accounttype<<"|"<<balance<<"|"<<pin<<endl;
         
    }
    void accountdetail(){
        cout<<"------------Account Detail----------------\n";
    string n;
    ifstream i("\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
    cout<<"ENTER YOUR NAME : ";
    cin>>n;
   while(true){
          getline(i, name, '|');
            if (!i) break;
             i >> Accountno; i.ignore();
            getline(i, accounttype, '|');
            i >> balance; i.ignore();
            i >> pin; i.ignore();
        if(name==n){
            cout<<"Name ="<<name<<endl;
            cout<<"Account Number:="<<Accountno<<endl;
            cout<<"Account type:=";
            cout<<"Balance:="<<balance<<endl;
            break;
        }
        else{
            cout<<"NAME NOT MATCHED:"<<endl;
            break;
        }
    }
    i.close();

}
    void loginaccount(){
          ifstream i("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
         ofstream o("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank1.txt");
        long long a;
        long p;
        bool found=false;
        cout<<"Enter Account Number:-";
        cin>>a;
        while(true){
          getline(i, name, '|');
            if (!i) break;
             i >> Accountno; i.ignore();
            getline(i, accounttype, '|');
            i >> balance; i.ignore();
            i >> pin; i.ignore();

        if(Accountno==a){
            found=true;
            cout<<"Enter Pin:-";
            cin>>p;
            if(p!=pin){
                cout<<"Wrong Pin\n";
               int choose;
              
               cout<<"1.forget pin\n";
               cout<<"2.exit...\n";
               cout<<"enter choose:";
               cin>>choose;
                if(choose==1){
                    long mobaile,newpin,cp;
               int otp=1234;
               int ot;
                    cout<<"enter your mobaile number:-";
                         cin>>mobaile;
                         cout<<"enter otp:-";
                         cin>>ot;
                        if(otp==ot){
                            cout<<"ENTER NEW PIN:-";
                            cin>>p;
                            cout<<"ENTER CONFIRM PIN:-";
                            cin>>cp;
                            if(p==cp){
                                pin=cp;
                                cout<<"PIN UPDATED SUCCESS...!"<<endl;  
                            }else{
                                cout<<"WRONG CONFIRM PIN.."<<endl;
                            }
                        }else{
                      cout<<"WRONG OTP.."<<endl;
                     }
                }
          }
         else{  
             int choose;
             do{
            cout<<"------------Transaction System--------------\n";
            cout<<"1.Diposit\n";
            cout<<"2.Withdraw\n";
            cout<<"3.Check balance\n";
            cout<<"4.Transaction history\n";
            cout<<"5.Exit\n";
            cout<<"Enter choise";
            cin>>choose;
          if(choose==1){
            cout<<"----------------Diposit--------------------\n";
             double amount;
             long p1;
               cout<<"Enter Amount:";
               cin>>amount;
             cout<<"Enter Pin";
             cin>>p1;
               if(p1==pin){
               balance+=amount;
               ofstream to("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\transac.txt",ios::app);
               to<<Accountno<<"|Debit|"<<amount<<"|"<<currentDateTime();
               to.close();
               cout<<"Diposit successfully\n";
               cout<<" Thank you \n";
               }
               else{
                cout<<"Wrong pin:\n";break;
           }
        }
    else if(choose==2){ 
        cout<<"-------------Withdraw----------------\n";
        long p1;
        double amount;  
             cout<<"Enter Amount";
             cin>>amount;
             if(balance>amount){
               cout<<"Enter Pin:-";
               cin>>p1;
               if(p1==pin){
                balance-=amount;
                 ofstream to("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\transac.txt",ios::app);
               to<<Accountno<<"|Credit|"<<amount<<"|"<<currentDateTime();
               to.close();
               cout<<"Withdraw Successfully!\n";
               }
               else{
                cout<<"wrong pin\n";break;
               }
             }
             else {
                cout<<"Low balance:\n";break;
                 }
    }
        else if(choose==3){
            cout<<"----------------Check Balance------------------\n";
       long p1;
     cout<<"Enter pin:-";
                 cin>>p1;
                 if(p1==pin){
                 cout<<" Current Balance:="<<balance<<endl;
                 }else{
                    cout<<"wrong pin:\n";
                 }
      
  }
 else if(choose==4){    
      ifstream ti("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\transac.txt");
    string type,time;
    long long a;
    long p1;
    double amount; 
    bool found=false;
    cout<<"\n----------Transaction History--------\n";
             cout<<"Enter pin";
             cin>>p1;
             if(p1==pin){
             while (true) {
                if(!ti)break;
              ti>>Accountno;ti.ignore();
            getline(ti, type, '|');
            ti >> amount; ti.ignore();
            getline(ti, time);
        cout<<Accountno<<"|"<<type<<"|"<<amount<<"|"<<time<<endl;
        }
        ti.close();
         
             }
             else {
                cout<<"Wrong pin:\n";break;
             }
     }
    }while(choose!=5);

            }

           }  
          o<<name<<"|"<<Accountno<<"|"<<accounttype<<"|"<<balance<<"|"<<pin<<endl;                 
    }
    i.close();
    o.close();
    remove("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
    rename("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank1.txt","C:\\Users\\dc\\Desktop\\project.cpp\\bank.txt");
    if(!found){
        cout<<"Account Not found!"<<endl;
    }
}
void searchaccount(){
    long long int a;
bool found=false;
    ifstream i("\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
    cout<<"------------Search Account------------\n";
    cout<<"ENTER YOUR ACCOUNT NUMBER : ";
    cin>>a;
    while(true){
          getline(i, name, '|');
            if (!i) break;
             i >> Accountno; i.ignore();
            getline(i, accounttype, '|');
            i >> balance; i.ignore();
            i >> pin; i.ignore();
        if(Accountno==a){
            cout<<"NAME ="<<name<<endl;
            cout<<"ACCOUNT NUMBER:="<<Accountno<<endl;
            cout<<"Account type:="<<accounttype<<endl;
            cout<<"BLANCE:="<<balance<<endl;
            break;
        }
        else{
            cout<<"Account not found:"<<endl;
            break;
        }
    }
    i.close();   
}
void updateaccountdetail(){
    long long int a,p;
    string n;
    bool  found=false;
    int choice;
    cout<<"===============Upadte Account Detail===============\n";
     ifstream i("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
    ofstream o("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank1.txt");
    cout<<" 1.UPDATE NAME \n";
    cout<<" 2.UPDATE PIN  \n";
    cout<<" 3. EXITE      \n";
    cout<<"ENTER YOUR CHOICE: ";
    cin>>choice;
        cout<<"ENTER YOUR ACCOUNT NUMBER:";
        cin>>a;
         while(true){
          getline(i, name, '|');
            if (!i) break;
             i >> Accountno; i.ignore();
            getline(i, accounttype, '|');
            i >> balance; i.ignore();
            i >> pin; i.ignore();
        if(Accountno==a){
            found=true;
            switch(choice){
                cout<<"--------Update Account Name------------\n";
         case 1: cout<<"  ENTER NEW NAME  : ";
               cin.ignore();
                 getline(cin,n); 
              name=n;
        cout<<"NAME UPDATE SUCCESS...!"<<endl;
        break;
case 2:
        if(Accountno==a){
        cout<<"-------Update Account pin------------\n";
            cout<<"ENTER YOUR PIN :";
            cin>>p;
            if(pin==p){
                cout<<"ENTER NEW PIN:";
                cin>>p;
                pin=p;
                cout<<"PIN UPDATE SUCCESS...!"<<endl;
            }
            else {
               cout<<"WRONG PIN.."<<endl;
               int choose;
               long long m,p,cp;
               int otp=1234;
               int o;
               cout<<"1.forget pin\n";
               cout<<"2.exit...\n";
               cout<<"enter choose:";
               cin>>choose;
                if(choose==1){
                    cout<<"enter your mobaile number:-";
                         cin>>m;
                         cout<<"enter otp:-";
                         cin>>o;
                        if(otp==o){
                            cout<<"ENTER NEW PIN:-";
                            cin>>p;
                            cout<<"ENTER CONFIRM PIN:-";
                            cin>>cp;
                            if(p==cp){
                                pin=cp;
                                cout<<"PIN UPDATED SUCCESS...!"<<endl;
                                
                                
                            }
                            else{
                                cout<<"WRONG CONFIRM PIN.."<<endl;
                            }

                        }
                        else{
                            cout<<"WRONG OTP.."<<endl;
                            
                }    
               }
            }       
            }
            break;
             case 3: cout<<"EXIT THE PROGRAM:"<<endl;break;
      default : 
         cout<<"INVALID CHOOSE..."<<endl;
       
        }
         o<<name<<"|"<<Accountno<<"|"<<accounttype<<"|"<<balance<<"|"<<pin<<endl; 
            }
    if(!found){
   cout<<"ACCOUNT NUMBER NOT FOUND\n";
    }
}
       i.close();
     o.close();
     remove("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
    rename("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank1.txt","C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
 
}  
void deleteaccount(){
    ifstream i("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
    ofstream o("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank1.txt");
    long long int acc;
    cout<<"ENTER ACCPOUNT NUMBER TO DELETE:";
    cin>>acc;
    bool found=false;
  while(true){
          getline(i, name, '|');
            if (!i) break;
             i >> Accountno; i.ignore();
            getline(i, accounttype, '|');
            i >> balance; i.ignore();
            i >> pin; i.ignore();
        if(Accountno==acc){
        found=true;
        continue;
        }
         o<<name<<"|"<<Accountno<<"|"<<accounttype<<"|"<<balance<<"|"<<pin<<endl; 
    }
      i.close();
     o.close();
    remove("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank.txt");
    rename("C:\\Users\\dc\\Desktop\\Banksystem-project.cpp\\bank1.txt","C:\\Users\\dc\\Desktop\\Banksystem-projec.cpp\\bank.txt");
     if (found){
        cout << " ACCOUNT DELETED SUCCESSFULLY\n";
     }
    else{
        cout << "ACCOUNT NOT FOUND\n";
    }
}
};
int main(){
    Bank b;
    int choice;
    do{
        cout<<"=============================================\n";
        cout<<"              BANK  ECO   SYSTEM  \n";
        cout<<"============================================\n";
        cout<<"1.creat account \n";
        cout<<"2.login --> (1.Diposit,2.Withdraw,3.Check blance, 4.Tansaction history)\n";
        cout<<"3.Search Account\n";
        cout<<"4.Update Account Detail\n";
        cout<<"5.Delete Account\n";
        cout<<"7.Account Detail\n";
        cout<<"6.Exit\n";
        cout<<"Enter choice :-";
        cin>>choice;
        switch(choice){
            case 1: b.Accountcreat();break;
            case 2: b.loginaccount();break;
            case 3: b.searchaccount();break;
            case 4: b.updateaccountdetail();break;
            case 5: b.deleteaccount();break;
            case 6: b.accountdetail();break;
            case 7: cout<<"EXIT"<<endl;
            default: 
            cout<<"Invalid choice:";
        }
    }while(choice!=7);
    
}
