#include <iostream>
#include <cstdio>
#include <stdio.h>
#include <string>
using namespace std;
#define MAX 99999999

int gTime = 0;
int health,N,R,K,T; //R=arrow攻擊力
int dragonH,ninjaH,icemanH,lionH,wolfH;
int dragonAtk,ninjaAtk,icemanAtk,lionAtk,wolfAtk;
string weapon[3]={"sword","bomb","arrow"};
string soldierType[5] = {"dragon","ninja","iceman","lion","wolf"};
int redSequence[5] = {2,3,4,1,0};
int blueSequence[5] = {3,0,1,2,4};
int redBeTaken=false;
int blueBeTaken=false;

//武士類
class soldier{
public:
    int id;
    int city;
    string color;
    string name;    //設置武士的名字
    int HP;         //武士的生命
    int atk;
    int sword=0;
    int arrow=0;
    int bomb=0;
    bool winFight=false;
    soldier(string color, int hp,string name,int totalSoldier,int Atk):color(color),HP(hp),name(name),id(totalSoldier),atk(Atk){
        printf("%s %s %d born\n",color.c_str(),name.c_str(),id);
    }
    void armed(int id){     //裝備武器
        if(weapon[id%3] == "sword"){
            sword = atk/5;
        }else if (weapon[id%3] == "bomb"){
            bomb = 1;
        }else{
            arrow = 3;
        }
    }
    int attack(soldier *s){
        if(s->HP > 0){  //敵人戰鬥前是存活的死亡
            printf("%03d:40 %s %s %d attacked %s %s %d in city %d with %d elements and force %d\n",gTime,color.c_str(),name.c_str(),id,s->color.c_str(),s->name.c_str(),s->id,city,HP,atk);
            s->beAttack(this);
            if(s->HP > 0){
                s->fightBack(this);
                if(HP<=0){
                    s->takeWeapon(this);    //wolf如果反擊殺掉自己，自己的武器被撿走
                    s->winFight=true;
                    return 2;//自己被反擊死
                }else{
                    yell(3);
                    loyalChange();
                    s->loyalChange();
                    return 3;//和局
                }
            }
        }
        yell(1);
        takeWeapon(s);      //wolf撿敵人武器
        winFight = true;
        return 1; //戰鬥勝利
    }
    virtual void fightBack(soldier *s){
        printf("%03d:40 %s %s %d fought back against %s %s %d in city %d\n",gTime,color.c_str(),name.c_str(),id,s->color.c_str(),s->name.c_str(),s->id,city);
        int temp=atk;
        atk /= 2;           //反擊只有攻擊力的一半
        s->beAttack(this);
        atk = temp;
    }
    virtual void beAttack(soldier *s){
        HP -= (s->atk + s->sword);
        s->sword *= 0.8;
        if(HP<=0){
            printf("%03d:40 %s %s %d was killed in city %d\n",gTime,color.c_str(),name.c_str(),id,city);
        }
    }
    virtual void yell(int n){};
    virtual bool runAway(){return false;};
    virtual void takeWeapon(soldier *s){};
    virtual void loyalChange(){};
    virtual void move(){
        if(color == "red"){
            city++;
        }else{
            city--;
        }
    };
    
};

class dragon:public soldier{
public:
    double morale;
    
    dragon(string color, int hp,int headquarterHP, int totalSoldier,int Atk):soldier(color,hp,"dragon",totalSoldier,Atk){
        morale = double(headquarterHP)/double(HP);
        armed(id%3);
        printf("Its morale is %.2lf\n",morale);
    }
    virtual void yell(int n){
        switch (n) {
            case 1:
                morale += 0.2;
                break;
            case 3:
                morale -= 0.2;
                break;
            default:
                break;
        }
        if(morale>0.8){
            printf("%03d:40 %s %s %d yelled in city %d\n",gTime,color.c_str(),name.c_str(),id,city);
        }
    }
};

class ninja:public soldier{
public:
    ninja(string color, int hp, int totalSoldier,int Atk):soldier(color,hp,"ninja",totalSoldier,Atk){
        armed(id%3);
        armed((id+1)%3);
    }
    virtual void fightBack(soldier *s){}
};

class iceman:public soldier{
public:
    int moveTemp=0;
    iceman(string color, int hp, int totalSoldier,int Atk):soldier(color,hp,"iceman",totalSoldier,Atk){
        armed(id%3);
    }
    virtual void move(){
        if(color == "red"){
            city++;
        }else{
            city--;
        }
        if(moveTemp!=0){
            if(HP>9){
                HP -=9;
                atk += 20;
            }else{
                HP = 1;
                atk += 20;
            }
            moveTemp = 0;
        }else{
            moveTemp ++;
        }
    }
};

class lion:public soldier{
    
    int loyalty;
public:
    lion(string color, int hp,int headquarterHP, int totalSoldier,int Atk):soldier(color,hp,"lion",totalSoldier,Atk){
        loyalty = headquarterHP;
        printf("Its loyalty is %d\n",loyalty);
    }
    virtual bool runAway(){
        if(loyalty <= 0){
            printf("%03d:05 %s %s %d ran away\n",gTime,this->color.c_str(),this->name.c_str(),this->id);
            return true;
        }else{
            return false;
        }
    }
    virtual void loyalChange(){
        loyalty -= K;
    }
    virtual void beAttack(soldier *s){
        int temp=HP;
        HP -= (s->atk + s->sword);
        s->sword *= 0.8;
        if(HP<=0){          //如果lion戰死，敵人增加lion戰鬥前的ＨＰ
            s->HP += temp;
            printf("%03d:40 %s %s %d was killed in city %d\n",gTime,color.c_str(),name.c_str(),id,city);
        }
    }
};

class wolf:public soldier{
public:
    wolf(string color, int hp, int totalSoldier,int Atk):soldier(color,hp,"wolf",totalSoldier,Atk){}
    virtual void takeWeapon(soldier *s){
        if(sword==0){
            sword = s->sword;
        }
        if(arrow==0){
            arrow = s->arrow;
        }
        if(bomb==0){
            bomb = s->bomb;
        }
    }
};

//城市
class city{
    
public:
    string flag;
    string flagTemp;
    int id;
    int element=0;
    soldier *redSoldier = NULL;
    soldier *blueSoldier = NULL;
    city(int id):id(id){}
    city(int id,int element):id(id),element(element){}
    
    void creatElement(){
        element += 10;
    }
    
    int ElementBetaken(){  //回收城市中的element
        int temp;
        temp = element;
        element = 0;
        return temp;
    };
    
    void putFlag(string color){
        if(flagTemp == color){
            if(flag != color && color != ""){
                flag = color;
                printf("%03d:40 %s flag raised in city %d\n",gTime,color.c_str(),id);
            }
        }else{
            flagTemp = color;
        }
    }
    
};

//總部類
class Headquarter:public city{
public:
    string color;   //總部的顏色
    int totalSoldier = 0; //總部的武士數量
    int soldierSequence[5];
    int soldierHPSequence[5];
    int soldierNumber[5]={0};  //記錄各個武士的數量
    
    Headquarter(string color,int hp,int cityID):color(color),city(cityID,hp){}
    void init(){
        for(int i=0;i<5;i++){
            switch (soldierSequence[i]) {
                case 0:
                    soldierHPSequence[i] = dragonH;
                    break;
                case 1:
                    soldierHPSequence[i] = ninjaH;
                    break;
                case 2:
                    soldierHPSequence[i] = icemanH;
                    break;
                case 3:
                    soldierHPSequence[i] = lionH;
                    break;
                case 4:
                    soldierHPSequence[i] = wolfH;
                    break;
                default:
                    printf("error");
                    break;
            }
        }
    }
    void soldierBorn(int sequence){   //總部生產武士
        
        soldier *temp = NULL;
        sequence = soldierSequence[sequence];
        soldierNumber[sequence]++;//該種類的武士數量+1
        totalSoldier++;         //總部內武士總數量+1
        
        printf("%03d:00 ",gTime);
        switch (sequence) {
            case 0:
                element -= dragonH;
                temp = new dragon(color,dragonH,element,totalSoldier,dragonAtk);
                break;
            case 1:
                element -= ninjaH;
                temp = new ninja(color,ninjaH,totalSoldier,ninjaAtk);
                break;
            case 2:
                element -= icemanH;
                temp = new iceman(color,icemanH,totalSoldier,icemanAtk);
                break;
            case 3:
                element -= lionH;
                temp = new lion(color,lionH, element, totalSoldier,lionAtk);
                break;
            case 4:
                element -= wolfH;
                temp = new wolf(color,wolfH,totalSoldier,wolfAtk);
                break;
            default:
                printf("error");
                break;
        }
        if(color == "red"){
            redSoldier = temp;
            redSoldier->city = 0;
        }else{
            blueSoldier = temp;
            blueSoldier->city = N+1;
        }
    }
};

//0:05 runAway
void runAwayCheck(city **citys){
    for(int i=0;i<=N+1;i++){
        if(citys[i]->redSoldier != NULL && citys[i]->redSoldier->runAway()){
            delete citys[i]->redSoldier;
            citys[i]->redSoldier = NULL;}
        if(citys[i]->blueSoldier != NULL && citys[i]->blueSoldier->runAway()){
            delete citys[i]->blueSoldier;
            citys[i]->blueSoldier = NULL;}
    }
}

//0:10 march
void march(city **citys){
    //red移動
    if(citys[N+1]->redSoldier != NULL && citys[N]->redSoldier != NULL){   //檢查blueHeadquarter是否被佔領
        blueBeTaken = true;
        citys[N+1]->flag = "red";
    }
    for(int i=0;i<=N;i++){              //從最靠近敵方總部的城市開始檢查，若該城市有red士兵，red士兵移動到下一個城市．
        if(citys[N-i]->redSoldier != NULL){
            citys[N-i]->redSoldier->move();
            citys[N-i+1]->redSoldier = citys[N-i]->redSoldier;
            citys[N-i]->redSoldier = NULL;
        }
    }
    
    //blue移動
    if(citys[0]->blueSoldier != NULL && citys[1]->blueSoldier != NULL){   //檢查redHeadquarter是否被佔領
        redBeTaken = true;
        citys[0]->flag = "blue";
    }
    for(int i=0;i<=N;i++){
        if(citys[i+1]->blueSoldier != NULL){
            citys[i+1]->blueSoldier->move();
            citys[i]->blueSoldier = citys[i+1]->blueSoldier;
            citys[i+1]->blueSoldier = NULL;
        }
    }
    
    //報告移動後狀況
    if((citys[0]->blueSoldier != NULL && citys[0]->flagTemp == "") || citys[0]->flag == "blue"){
        printf("%03d:10 blue %s %d reached red headquarter with %d elements and force %d\n",gTime,citys[0]->blueSoldier->name.c_str(),citys[0]->blueSoldier->id,citys[0]->blueSoldier->HP,citys[0]->blueSoldier->atk);
        citys[0]->flagTemp = "blue";
    }
    if(redBeTaken){
        printf("%03d:10 red headquarter was taken\n",gTime);
    }
    
    for(int i=1;i<=N;i++){
        if(citys[i]->redSoldier != NULL){
            printf("%03d:10 red %s %d marched to city %d with %d elements and force %d\n",gTime,citys[i]->redSoldier->name.c_str(),citys[i]->redSoldier->id,citys[i]->redSoldier->city,citys[i]->redSoldier->HP,citys[i]->redSoldier->atk);
        }
        if(citys[i]->blueSoldier != NULL){
            printf("%03d:10 blue %s %d marched to city %d with %d elements and force %d\n",gTime,citys[i]->blueSoldier->name.c_str(),citys[i]->blueSoldier->id,citys[i]->blueSoldier->city,citys[i]->blueSoldier->HP,citys[i]->blueSoldier->atk);
        }
    }
    
    if((citys[N+1]->redSoldier != NULL && citys[N+1]->flagTemp == "") || citys[N+1]->flag == "red"){
        printf("%03d:10 red %s %d reached blue headquarter with %d elements and force %d\n",gTime,citys[N+1]->redSoldier->name.c_str(),citys[N+1]->redSoldier->id,citys[N+1]->redSoldier->HP,citys[N+1]->redSoldier->atk);
        citys[N+1]->flagTemp = "red";
    }
    if(blueBeTaken){
        printf("%03d:10 blue headquarter was taken\n",gTime);
    }
}

//0:30 soldier takes citys element
void soldierTakesElement(city **citys){
    for(int i=1;i<=N;i++){
        if(citys[i]->redSoldier != NULL && citys[i]->blueSoldier == NULL){
            printf("%03d:30 red %s %d earned %d elements for his headquarter\n",gTime,citys[i]->redSoldier->name.c_str(),citys[i]->redSoldier->id,citys[i]->element);
            citys[0]->element += citys[i]->ElementBetaken();
        }else if (citys[i]->blueSoldier != NULL && citys[i]->redSoldier == NULL){
            printf("%03d:30 blue %s %d earned %d elements for his headquarter\n",gTime,citys[i]->blueSoldier->name.c_str(),citys[i]->blueSoldier->id,citys[i]->element);
            citys[N+1]->element += citys[i]->ElementBetaken();
        }
    }
}

//0:35 shootArrow
void shootArrow(city **citys){
    for(int i=1;i<=N;i++){
        if(i!= N && citys[i]->redSoldier!=NULL && citys[i]->redSoldier->arrow!=0 && citys[i+1]->blueSoldier!=NULL){
            citys[i]->redSoldier->arrow -=1;
            citys[i+1]->blueSoldier->HP -= R;
            if(citys[i+1]->blueSoldier->HP <=0){
                printf("%03d:35 red %s %d shot and killed blue %s %d\n",gTime,citys[i]->redSoldier->name.c_str(),citys[i]->redSoldier->id,citys[i+1]->blueSoldier->name.c_str(),citys[i+1]->blueSoldier->id);
            }else{
                printf("%03d:35 red %s %d shot\n",gTime,citys[i]->redSoldier->name.c_str(),citys[i]->redSoldier->id);
            }
        }
        if (i!= 1 && citys[i]->blueSoldier!=NULL && citys[i]->blueSoldier->arrow!=0 && citys[i-1]->redSoldier!=NULL){
            citys[i]->blueSoldier->arrow -=1;
            citys[i-1]->redSoldier->HP -= R;
            if(citys[i-1]->redSoldier->HP <=0){
                printf("%03d:35 blue %s %d shot and killed red %s %d\n",gTime,citys[i]->blueSoldier->name.c_str(),citys[i]->blueSoldier->id,citys[i-1]->redSoldier->name.c_str(),citys[i-1]->redSoldier->id);
            }else{
                printf("%03d:35 blue %s %d shot\n",gTime,citys[i]->blueSoldier->name.c_str(),citys[i]->blueSoldier->id);
            }
        }
    }
}

//0:38 useBomb
void useBomb(soldier **atkSoldier,soldier **defSoldier){
    soldier *def = *defSoldier;
    soldier *atk = *atkSoldier;
    
    if(def->HP<=0 || atk->HP<=0){   //如果雙方有人之前已經被箭射死了，就都不使用炸彈
        return;
    }
    
    if(def->bomb>0 && def->HP <= (atk->atk+atk->sword)){
        def->bomb -= 1;
        printf("%03d:38 %s %s %d used a bomb and killed %s %s %d\n",gTime,def->color.c_str(),def->name.c_str(),def->id,atk->color.c_str(),atk->name.c_str(),atk->id);
        delete *atkSoldier;
        *atkSoldier = NULL;
        delete def;
        *defSoldier = NULL;
    }else if (def->HP > (atk->atk+atk->sword) && atk->bomb>0 && atk->HP <= (def->atk/2+def->sword) && def->name != "ninja" ){
        printf("%03d:38 %s %s %d used a bomb and killed %s %s %d\n",gTime,atk->color.c_str(),atk->name.c_str(),atk->id,def->color.c_str(),def->name.c_str(),def->id);
        delete *atkSoldier;
        *atkSoldier = NULL;
        delete *defSoldier;
        *defSoldier = NULL;
    }
}

//0:40 soldier win fight and takes citys element
void soldierWinsElement(city *city,Headquarter *red,Headquarter *blue){
    if(city->redSoldier != NULL && city->blueSoldier == NULL){
        printf("%03d:40 red %s %d earned %d elements for his headquarter\n",gTime,city->redSoldier->name.c_str(),city->redSoldier->id,city->element);
    }else if (city->blueSoldier != NULL && city->redSoldier == NULL){
        printf("%03d:40 blue %s %d earned %d elements for his headquarter\n",gTime,city->blueSoldier->name.c_str(),city->blueSoldier->id,city->element);
    }
}

//0:40 Headquarter give reward and takes elements from fight
void HeadquarterGiveReward(city **citys){
    
    //將戰鬥輸的士兵殺掉
    for(int i=1;i<=N;i++){
        if(citys[i]->redSoldier != NULL && citys[i]->redSoldier->HP <= 0){
            delete citys[i]->redSoldier;
            citys[i]->redSoldier = NULL;
        }else if (citys[i]->blueSoldier != NULL && citys[i]->blueSoldier->HP <= 0){
            delete citys[i]->blueSoldier;
            citys[i]->blueSoldier = NULL;
        }
    }
    
    //從最靠近敵方總部的城市開始檢查，獎勵戰鬥中勝利的red士兵．
    for(int i=0;i<=N;i++){
        if(citys[N-i]->redSoldier != NULL && citys[N-i]->redSoldier->winFight && citys[0]->element>8){
            citys[N-i]->redSoldier->HP += 8;
            citys[0]->element -= 8;
            citys[N-i]->redSoldier->winFight = false;}}
    //從最靠近敵方總部的城市開始檢查，獎勵戰鬥中勝利的blue士兵．
    for(int i=0;i<=N;i++){
        if(citys[i+1]->blueSoldier != NULL && citys[i+1]->blueSoldier->winFight && citys[N+1]->element>8){
            citys[i+1]->blueSoldier->HP += 8;
            citys[N+1]->element -=8;
            citys[i+1]->blueSoldier->winFight = false;}}
    //回收戰鬥勝利可以拿到的城市生命元
    for(int i=1;i<=N;i++){
        if(citys[i]->redSoldier != NULL && citys[i]->blueSoldier == NULL){
            citys[0]->element += citys[i]->ElementBetaken();
        }else if (citys[i]->blueSoldier != NULL && citys[i]->redSoldier == NULL){
            citys[N+1]->element += citys[i]->ElementBetaken();
        }
    }
}

//0:50 Weapon report
void weaponReport(soldier *s){
    int temp=0;
    printf("%03d:55 %s %s %d has ",gTime,s->color.c_str(),s->name.c_str(),s->id);
    if(s->arrow==0 && s->bomb==0 && s->sword==0){
        printf("no weapon\n");
    }else{
        if(s->arrow != 0){
            printf("arrow(%d)",s->arrow);
            temp = 1;
        }
        if(s->bomb != 0){
            if(temp){
                printf(",");
            }
            temp = 1;
            printf("bomb");
        }
        if(s->sword){
            if(temp){
                printf(",");
            }
            printf("sword(%d)",s->sword);
        }
        printf("\n");
    }
    
}

int main() {
    
    int cycle;
    cin >> cycle;
    
    Headquarter *red,*blue;
    city **citys;
    
    int redCount=0;
    int redCountTemp=0; //用來記錄目前總部生命值是否可以生產武士
    int blueCount=0;
    int blueCountTemp=0;
    
    for(int i=0;i<cycle;i++){
        
        
        
        cin >> health >> N >> R >> K >> T ;
        cin >> dragonH >> ninjaH >> icemanH >> lionH >> wolfH;
        cin >> dragonAtk >> ninjaAtk >> icemanAtk >> lionAtk >> wolfAtk;
        red = new Headquarter("red",health,0);
        blue = new Headquarter("blue",health,N+1);
        citys = new city*[22];
        
        //建造city
        citys[0] = red;
        citys[N+1] = blue;
        for(int i=1;i<=N;i++){
            citys[i] = new city(i);
        }
        
        
        //製造紅色總部，填入武士生產順序
        red->soldierSequence[0] = redSequence[0];
        red->soldierSequence[1] = redSequence[1];
        red->soldierSequence[2] = redSequence[2];
        red->soldierSequence[3] = redSequence[3];
        red->soldierSequence[4] = redSequence[4];
        red->init();
        
        //製造藍色總部，填入武士生產順序
        blue->soldierSequence[0] = blueSequence[0];
        blue->soldierSequence[1] = blueSequence[1];
        blue->soldierSequence[2] = blueSequence[2];
        blue->soldierSequence[3] = blueSequence[3];
        blue->soldierSequence[4] = blueSequence[4];
        blue->init();
        
        //reset
        redCount=0;
        redCountTemp=0;
        blueCount=0;
        blueCountTemp=0;
        gTime = 0;
        
        printf("Case %d:\n",i+1);
        
        while(true){
            
            //0:00 soldier create
            if(red->element >= red->soldierHPSequence[redCount%5]){
                red->soldierBorn(redCount%5);
                redCount++;}
            if(blue->element >= blue->soldierHPSequence[blueCount%5]){
                blue->soldierBorn(blueCount%5);
                blueCount++;}
            if(T<=0){break;}
            
            //0:05 run away
            T -= 5;
            if(T<0){break;}
            runAwayCheck(citys);
            
            
            //0:10 march
            T -= 5;
            if(T<0){break;}
            march(citys);
            if(redBeTaken || blueBeTaken){
                redBeTaken=false;
                blueBeTaken=false;
                break;}
            
            //0:20 element create
            T -= 10;
            if(T<0){break;}
            for(int i=1;i<=N;i++){
                citys[i]->creatElement();}
            
            
            //0:30 soldier takes citys element
            T -= 10;
            if(T<0){break;}
            soldierTakesElement(citys);
            
            
            //0:35 arrow
            T -= 5;
            if(T<0){break;}
            shootArrow(citys);
            
            
            //0:38 bomb
            T -= 3;
            if(T<0){break;}
            for(int i=1;i<=N;i++){
                if(citys[i]->redSoldier != NULL && citys[i]->blueSoldier != NULL){
                    if(citys[i]->flag == "red" || (citys[i]->flag != "blue" && i%2==1)){
                        useBomb(&citys[i]->redSoldier, &citys[i]->blueSoldier);
                    }else{
                        useBomb(&citys[i]->blueSoldier, &citys[i]->redSoldier);
                    }
                }
            }
            
            
            //0:40 fight
            T -= 2;
            if(T<0){break;}
            for(int i=1;i<=N;i++){
                if(citys[i]->redSoldier != NULL && citys[i]->blueSoldier != NULL){
                    if(citys[i]->flag == "red" || (citys[i]->flag != "blue" && i%2==1)){
                        if(citys[i]->redSoldier->HP <= 0){  //開戰前自己就被射死了
                            if(citys[i]->blueSoldier->HP <= 0){     //檢查敵人是否也被射死
                                delete citys[i]->blueSoldier;
                                citys[i]->blueSoldier = NULL;
                                delete citys[i]->redSoldier;
                                citys[i]->redSoldier = NULL;
                            }else{      //如果敵人沒有死，那算敵人獲勝
                                citys[i]->blueSoldier->takeWeapon(citys[i]->redSoldier);      //wolf撿敵人武器
                                citys[i]->blueSoldier->winFight = true;
                                delete citys[i]->redSoldier;
                                citys[i]->redSoldier = NULL;
                                soldierWinsElement(citys[i],red,blue);
                                citys[i]->putFlag("blue");
                            }
                        }else{
                            switch(citys[i]->redSoldier->attack(citys[i]->blueSoldier)){
                                case 1:
                                    delete citys[i]->blueSoldier;
                                    citys[i]->blueSoldier = NULL;
                                    soldierWinsElement(citys[i],red,blue);
                                    citys[i]->putFlag("red");
                                    break;
                                case 2:
                                    delete citys[i]->redSoldier;
                                    citys[i]->redSoldier = NULL;
                                    soldierWinsElement(citys[i],red,blue);
                                    citys[i]->putFlag("blue");
                                    break;
                                case 3:
                                    citys[i]->putFlag("");
                                    break;
                            }
                        }
                    }else{
                        if(citys[i]->blueSoldier->HP <= 0){
                            if(citys[i]->redSoldier->HP <= 0){
                                delete citys[i]->redSoldier;
                                citys[i]->redSoldier = NULL;
                                delete citys[i]->blueSoldier;
                                citys[i]->blueSoldier = NULL;
                            }else{
                                citys[i]->redSoldier->takeWeapon(citys[i]->blueSoldier);
                                citys[i]->redSoldier->winFight = true;
                                delete citys[i]->blueSoldier;
                                citys[i]->blueSoldier = NULL;
                                soldierWinsElement(citys[i],red,blue);
                                citys[i]->putFlag("red");
                            }
                        }else{
                            switch(citys[i]->blueSoldier->attack(citys[i]->redSoldier)){
                                case 1:
                                    delete citys[i]->redSoldier;
                                    citys[i]->redSoldier = NULL;
                                    soldierWinsElement(citys[i],red,blue);
                                    citys[i]->putFlag("blue");
                                    break;
                                case 2:
                                    delete citys[i]->blueSoldier;
                                    citys[i]->blueSoldier = NULL;
                                    soldierWinsElement(citys[i],red,blue);
                                    citys[i]->putFlag("red");
                                    break;
                                case 3:
                                    citys[i]->putFlag("");
                                    break;
                            }
                        }
                    }
                }
            }
            HeadquarterGiveReward(citys);
            
            
            
            
            //0:50 headquarter report
            T -= 10;
            if(T<0){break;}
            printf("%03d:50 %d elements in red headquarter\n",gTime,red->element);
            printf("%03d:50 %d elements in blue headquarter\n",gTime,blue->element);
            
            
            
            //0:55 soldier weapon report
            T -= 5;
            if(T<0){break;}
            for(int i=0;i<=N+1;i++){
                if(citys[i]->redSoldier != NULL){
                    weaponReport(citys[i]->redSoldier);
                }
            }
            for(int i=0;i<=N+1;i++){
                if(citys[i]->blueSoldier != NULL){
                    weaponReport(citys[i]->blueSoldier);
                }
            }
            
            
            T -= 5;
            if(T<0){break;}
            gTime++;
        }
        
        delete red;
        delete blue;
        for(int i=1;i<=N;i++){
            delete citys[i];
        }
        delete [] citys;
        
    }
    
    
    return 0;
}
