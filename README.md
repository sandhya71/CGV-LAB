# CGV-LAB
4 AND 5  PEOGRAMS

#4.Draw a color cube and allow the user to move the camera suitably to experiment with perspective viewing 

#include<stdlib.h>
#include<glut.h> 
GLfloat vertices[][3]={{-1.0,-1.0,-1.0},{1.0,-1.0,-1.0},
{1.0,1.0,-1.0},{-1.0,1.0,-1.0},{-1.0,-1.0,1.0},
{1.0,-1.0,1.0},{1.0,1.0,1.0},{-1.0,1.0,1.0}}; 
Glfloat colors[][3]={{0.0,0.0,0.0},{1.0,0.0,0.0}, 
{1.0,1.0,0.0},{0.0,1.0,0.0},{0.0,0.0,1.0}, 
{1.0,0.0,1.0},{1.0,1.0,1.0},{0.0,1.0,1.0}}; 
 
void polygon(int a, int b, int c, int d)
{  
    glBegin(GL_POLYGON);  
    glColor3fv(colors[a]); 
    glVertex3fv(vertices[a]); 
    glColor3fv(colors[b]);  
    glVertex3fv(vertices[b]);  
    glColor3fv(colors[c]);         
    glVertex3fv(vertices[c]); 
    glColor3fv(colors[d]); 
    glVertex3fv(vertices[d]); 
    glEnd(); } 
 
void colorcube() 
{ 
 polygon(0,3,2,1); 
 polygon(2,3,7,6);
 polygon(0,4,7,3);
 polygon(1,2,6,5);
 polygon(4,5,6,7);  
 polygon(0,1,5,4);
} 
 
static GLfloat theta[]={0.0,0.0,0.0}; static GLint axis=2; static GLdouble viewer[]={0.0,0.0,5.0}; 
 
void display(void)
{
   glClear(GL_COLOR_BUFFER_BIT|GL_DEPTH_BUFFER_BIT);  
   glLoadIdentity(); 
   gluLookAt(viewer[0], viewer[1],viewer[2],0.0,0.0,0.0,0.0,1.0,0.0); 
 glRotatef(theta[0],1.0,0.0,0.0);
 glRotatef(theta[1],0.0,1.0,0.0); 
 glRotatef(theta[2],0.0,0.0,1.0);  
 colorcube();  
 glFlush(); 
 glutSwapBuffers(); } 
 
void mouse(int btn, int state, int x, int y) 
{  
   if(btn==GLUT_LEFT_BUTTON && state==GLUT_DOWN) axis=0;  
   if(btn==GLUT_MIDDLE_BUTTON&&state==GLUT_DOWN) axis=1;  
   if(btn==GLUT_RIGHT_BUTTON&&state==GLUT_DOWN) axis=2;  
   theta[axis]+=2.0;  
   if(theta[axis]>360.0) theta[axis]-=360.0; 
   display();
 } 
 
void keys(unsigned char key, int x, int y)
{  
   if(key == 'x') viewer[0]-=1.0; 
   if(key == 'X') viewer[0]+=1.0;  
   if(key == 'y') viewer[1]-=1.0;  
   if(key == 'Y') viewer[1]+=1.0; 
   if(key == 'z') viewer[2]-=1.0; 
   if(key == 'Z') viewer[2]+=1.0;  
   display(); 
} 
 
void myReshape(int w, int h)
{ 
   glViewport(0,0,w,h);  
   glMatrixMode(GL_PROJECTION);  
   glLoadIdentity();  
   if(w<=h)  
   glFrustum(-2.0,2.0,-2.0*(GLfloat)h/(GLfloat)w,  2.0*(GLfloat)h/(GLfloat)w, 2.0,20.0); 
   else
   glFrustum(-2.0,2.0,-2.0*(GLfloat)w/(GLfloat)h,  2.0*(GLfloat)w/(GLfloat)h, 2.0,20.0);
   glMatrixMode(GL_MODELVIEW); 
 } 
 
void main(int argc, char **argv)
{ 
  glutInit(&argc, argv);  
  glutInitDisplayMode(GLUT_DOUBLE|GLUT_RGB|GLUT_DEPTH);  
  glutInitWindowSize(500,500); 
  glutCreateWindow("Colorcube Viewer");
  glutReshapeFunc(myReshape); 
  glutDisplayFunc(display); 
  glutMouseFunc(mouse); 
  glutKeyboardFunc(keys); 
  glEnable(GL_DEPTH_TEST);
  glutMainLoop();
 }
 
 #5. Clip a lines using Cohen-Sutherland algorithm 
 
#include <stdio.h>
#include <GL/glut.h> 
 
double xmin=50,ymin=50, xmax=100,ymax=100; 
double xvmin=200,yvmin=200,xvmax=300,yvmax=300; 
  const int RIGHT = 8; 
  const int LEFT = 2;
  const int TOP = 4; 
  const int BOTTOM = 1; 
  outcode ComputeOutCode (double x, double y); 
  
void CohenSutherlandLineClipAndDraw (double x0, double y0,double x1, double y1)
{   
   outcode outcode0, outcode1, outcodeOut;      
   bool accept = false, done = false; 
   outcode0 = ComputeOutCode (x0, y0); 
   outcode1 = ComputeOutCode (x1, y1);   
   do{     
        if (!(outcode0 | outcode1))   
        {           
        accept = true;        
        done = true; 
         else if (outcode0 & outcode1) 
          done = true;              
          else  
          {
           double x, y; 
           outcodeOut = outcode0? outcode0: outcode1; 
             if (outcodeOut & TOP) 
              {      
              x = x0 + (x1 - x0) * (ymax - y0)/(y1 - y0); 
              y = ymax;    
              }                        
              else
              if (outcodeOut & BOTTOM)
               {                  
               x = x0 + (x1 - x0) * (ymin - y0)/(y1 - y0); 
               y = ymin; 
               }
               else if (outcodeOut & RIGHT)   
               {                             
               y = y0 + (y1 - y0) * (xmax - x0)/(x1 - x0);   
               x = xmax; 
               }
               else                           
               { 
               y = y0 + (y1 - y0) * (xmin - x0)/(x1 - x0);         
               x = xmin;   
               }
                              
                x0 = x;          
                y0 = y;    
                outcode0 = ComputeOutCode (x0, y0);       
                }
                else     
                {        
                x1 = x;
                y1 = y;  
                outcode1 = ComputeOutCode (x1, y1);   
                }  
                } 
                }
                while (!done); 
        if (accept)      
        {
         double sx=(xvmax-xvmin)/(xmax-xmin); 
         double sy=(yvmax-yvmin)/(ymax-ymin);    
         double vx0=xvmin+(x0-xmin)*sx;   
         double vy0=yvmin+(y0-ymin)*sy;       
         double vx1=xvmin+(x1-xmin)*sx;         
         double vy1=yvmin+(y1-ymin)*sy;                         
         glColor3f(1.0, 0.0, 0.0);       
         glBegin(GL_LINE_LOOP);   
         glVertex2f(xvmin, yvmin);        
         glVertex2f(xvmax, yvmin);
         glVertex2f(xvmax, yvmax);
         glVertex2f(xvmin, yvmax);          
         glEnd(); 
        glColor3f(0.0,0.0,1.0); 
        glBegin(GL_LINES); 
        glVertex2d (vx0, vy0);    
        glVertex2d (vx1, vy1);
        glEnd(); 
        }
        } 
        outcode ComputeOutCode (double x, double y)
        {        
        outcode code = 0;     
        if (y > ymax)             
        code |= TOP;    
        else if (y < ymin)        
        code |= BOTTOM;  
        if (x > xmax)             
        code |= RIGHT;        
        else if (x < xmin)        
        code |= LEFT;       
        return code; 
        } 
 
void display() 
{
double x0=60,y0=20,x1=80,y1=120; 
glClear(GL_COLOR_BUFFER_BIT);
glColor3f(1.0,0.0,0.0);
glBegin(GL_LINES);      
glVertex2d (x0, y0);  
glVertex2d (x1, y1);       
glEnd(); 

glColor3f(0.0, 0.0, 1.0);  
glBegin(GL_LINE_LOOP);   
glVertex2f(xmin, ymin);
glVertex2f(xmax, ymin);
glVertex2f(xmax, ymax);   
glVertex2f(xmin, ymax);
glEnd(); 
CohenSutherlandLineClipAndDraw(x0,y0,x1,y1);
glFlush();
}

void myinit()
{
glClearColor(1.0,1.0,1.0,1.0);     
glColor3f(1.0,0.0,0.0);
glPointSize(1.0); 
glMatrixMode(GL_PROJECTION);    
glLoadIdentity(); 
gluOrtho2D(0.0,499.0,0.0,499.0);
}
void main(int argc, char** argv)
{ 
glutInit(&argc,argv);      
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);        
glutInitWindowSize(500,500);   
glutInitWindowPosition(0,0);
glutCreateWindow("Cohen Suderland Line Clipping Algorithm");     
glutDisplayFunc(display); 
myinit();   
glutMainLoop(); 
} 
 
                
