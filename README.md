Sky s;
Star st;
Needles n;
int timer;

void setup(){
  size(1120, 630);
  s = new Sky(0, 0, 1, 0.8);
  st=new Star(width/2,height/2);
  n=new Needles(50);
  s.generateStars(0, 0, width, height);
  timer = 0;
}

void draw() {
  timer++;
  s.move();
  s.render();
  n.render();
  st.render();
}






class Needles{
  Needle[] ne;
  int increment;
  Needles(int amount){
    ne=new Needle[amount];
    for(int i=0;i<ne.length;i++){
      ne[i]=new Needle(st.x,st.y,st.xV,st.yV);
    }
    increment=0;
  }
  
  void render(){
    increment++;
    for(int i=0;i<ne.length;i++){
      if((int)(increment/10)==i){
        ne[i]=new Needle(st.x,st.y,st.xV,st.yV);
      }
    }
    for(int i=0;i<ne.length;i++){
      if((int)(increment/10)>=i){
        if(ne[i].rerender==true){
          ne[i]=new Needle(st.x,st.y,st.xV,st.yV);
        }
        ne[i].render();
      }
    }
    
  }
  
}


class Needle{
  float x,y,xVe,yVe;
  int direction,transparency;
  boolean rerender=false;
  Needle(float x1,float y1,float xv,float yv){
    x=x1;
    y=y1;
    xVe=-xv*((float)(Math.random())+.5);
    yVe=-yv*((float)(Math.random())+.5);
    direction=(int)(Math.random()*4);
    transparency=100;
  }
  
  void move(){
    x+=xVe;
    y+=yVe;
  }
  
   void show(){
     transparency--;
     if(transparency<0){
       rerender=true;
     }
     stroke(0,255,0,transparency);
     strokeWeight(2);
     if(direction==0){
       line(x+3,y,x-3,y); 
     }else if(direction==1){
       line(x+3,y+3,x-3,y-3); 
     }else if(direction==2){
       line(x,y+3,x,y-3); 
     }else if(direction==3){
       line(x-3,y+3,x+3,y-3); 
     }
   }
   
   void render(){
     move();
     show();
   }
  
}






class Sky {
  float xPos, yPos, xVel, yVel, frequency;
  ArrayList<SkyStar> skyStars = new ArrayList<SkyStar>();
  Sky(float xP, float yP, float xV, float yV) {
    xPos = xP;
    yPos = yP;
    xVel = xV;
    yVel = yV;
    frequency = 1000;
  }
  void generateStars(float x, float y, float wid, float hei) {
    for (int i = 0; i < wid * hei / frequency; i++) {
      if (Math.random() * frequency < wid * hei) {
        skyStars.add(new SkyStar(x + wid * (float)Math.random(), y + hei * (float)Math.random()));
      }
    }
  }
  void unloadStars() {
    if (timer % 10 == 0) {
      SkyStar s;
      for (int i = skyStars.size() - 1; i > -1; i--) {
        s = skyStars.get(i);
        if (s.xPos < xPos || s.yPos < yPos || s.xPos > xPos + width || s.yPos > yPos + height) {
          skyStars.remove(i);
        }
      }
    }
  }
  void move() {
    generateStars(xPos - Math.abs(xVel), yPos - Math.abs(yVel), Math.abs(xVel), height);
    generateStars(xPos, yPos - Math.abs(yVel), width, Math.abs(yVel));
    generateStars(xPos - Math.abs(xVel), yPos + height, width, Math.abs(yVel));
    generateStars(xPos + width, yPos, Math.abs(xVel), height);
    System.out.println(" ");
    xPos += xVel;
    yPos += yVel;
    unloadStars();
  }
  void render() {
    noStroke();
    fill(#222266);
    rect(0, 0, width, height);
    fill(#ffffff);
    for (int i = 0; i < skyStars.size(); i++) {
      skyStars.get(i).render();
    }
    stroke(#000000);
  }
}

class SkyStar {
  float xPos, yPos;
  boolean shine;
  SkyStar(float x, float y) {
    xPos = x;
    yPos = y;
  }
  void render() {
    rect(xPos - s.xPos, yPos - s.yPos, 2, 2);
  }
}






class Star{
  float x,y,xV,yV,yA,xA,timer;
  Star(float Startx,float Starty){
    x=Startx;
    y=Starty;
    xV=(float)(Math.random()*2)-1;
    yV=(float)(Math.random()*2)-1;
    xA=(float)(Math.random()*.1)-.05;
    yA=(float)(Math.random()*.1)-.05;
    timer=100;
  }
  
  float getXVel(){
    return xV;
  }
  
  float getYVel(){
    return yV;
  }
  
  
  void move(){
    timer--;
    if(timer<=0){
      timer=100;
      xA=(float)(Math.random()*.1)-.05;
      yA=(float)(Math.random()*.1)-.05;
    }
    xV+=xA;
    if(xV>=2){
      xV=2;
    }else if(xV<=-2){
      xV=-2;
    }
    x+=xV;
    yV+=yA;
    if(yV>=2){
      yV=2;
    }else if(yV<=-2){
      yV=-2;
    }
    y+=yV;
    
    if(x<=100){
      xA=.25;
    }else if(x>=width-100){
      xA=-.25;
    }
    if(y<=100){
      yA=.25;
    }else if(y>=height-100){
      yA=-.25;
    }
  }
  
  void show(){
    fill(255,255,0);
    noStroke();
    triangle(x,y,x+26,y,x-12,y-12);
    triangle(x,y,x+26,y,x-12,y+12);
    triangle(x,y,x+12,y-20,x+12,y+20);
    triangle(x+26,y,x,y+5,x,y-5);
  }
  
  void render(){
    move();
    show();
  }
  
}
