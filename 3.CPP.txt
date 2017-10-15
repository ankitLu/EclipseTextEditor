#include<iostream.h>
#include<conio.h>
#include<stdio.h>
#include<fstream.h>
#include<process.h>
#include<dos.h>
#include<string.h>
#include<graphics.h>
#define max 2000

int x,y;
FILE *fp;
ofstream outfile;
ifstream infile;
////////////////////////////////////
////// creating a class ////////////
////////////////////////////////////


class file

	{  char ch,line[max],oldname[10],newname[10];
	   char fname[10],fname1[10],fname2[10];

	   public:

	   void create();
	   void saveas();
	   void delt();
	   void open();
	   void open_ro();
	   void open_ed();
	   void renam();
	   void quit();
	   void help();
	   void cat();
	}f1;

//////////  end of class /////////////

//////////////////////////////////////
///// MOUSE INITIALIZATION ///////////
//////////////////////////////////////

union REGS in, out;
void detectmouse ()
 {
  cleardevice();
  in.x.ax=1;
  int86 (0X33,&in,&out);
  if (out.x.ax == 0)
  cout<<"\nMouse Fail To Initialize";
 }

void showmouse()
 {
   in.x.ax = 1;
   int86 (0X33,&in,&out);
 }

void hidemouse()
 { in.x.ax=2;
   int86 (0X33,&in,&out);
 }
///////////////////////////////////////////////////////
////////// CLASS MEMBER FUNCTIONS DEFINITION //////////
///////////////////////////////////////////////////////


void file :: create()
{hidemouse();
 cleardevice();
 outtextxy(30,10,"enter the name of the file to be created : ");
 gotoxy(5,3);
 gets(fname);
 fp=fopen(fname,"r");
 if(fp)
 {
  cleardevice();
  outtextxy(30,10," unable to create file");
  outtextxy(30,30," file already exists or file creation error");
  getch();
  f1.cat();
 }
 else
 {
  cleardevice();
  outfile.open(fname);
  outtextxy(30,10,"enter text and terminate with @");
  gotoxy(5,3);
  cin.getline(line,max,'@');
  cleardevice();

  outtextxy(30,10,"entering text into file....");
  cleardevice();
  outtextxy(30,10,"do you want to save the file?");
  outtextxy(30,25,"y / n");
  gotoxy(10,5);
  cin>>ch;
  if (ch=='y'||ch=='Y')
  {
   cleardevice();
   outfile<<line;
   cleardevice();
   outtextxy(30,10,"file has been saved");
   outfile.close();
   getch();
   f1.cat();
  }

  else
  {
   if(ch=='n'||ch=='N')
   {
    cleardevice();
    outtextxy(30,10,"file has not been saved");
    if (remove(fname)==0)
    getch();
    f1.cat();
   }
   }
 }
}
//////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////
void file::cat()
 {
  cleardevice();
  detectmouse();
  showmouse();
  settextstyle(7,0,15);
  outtextxy(30,150,"ECLIPSE");
  settextstyle(2,0,5);
  rectangle(2,30,637,470);
  rectangle(0,27,637,  0);
  setfillstyle(2,4);
  bar( 10,7,20,25);
  bar( 70,7,80,25);
  bar( 135,7,145,25);
  bar( 200,7,210,25);
  setfillstyle(1,15);
  bar( 20,7,50, 25);
  bar( 80,7,115, 25);
  bar(145,7,180,25);
  bar(210,7,245,25);
  setcolor(1);
  outtextxy( 25,10,"NEW");
  outtextxy( 83,10,"OPEN");
  outtextxy(147,10,"HELP");
  outtextxy(212,10,"EXIT");

  while ( !kbhit () )
  {
   in.x.ax = 3;
   int86 (0X33,&in,&out);
   x=out.x.cx;
   y=out.x.dx;

   if(out.x.bx==1)
    {
     in.x.ax = 3;
     int86 (0X33,&in,&out);
     x=out.x.cx;
     y=out.x.dx;

     if(x>=10&&x<=20&&y>=7&&y<=25)
     {
	    f1.create();
     }

     else if(x>=70&&x<=80&&y>=7&&y<=25)
     {
	    f1.open();
     }

     else if(x>=135&&x<=145&&y>=7&&y<=25)
     {
	    f1.help();
     }

     else if(x>=200&&x<=210&&y>=7&&y<=25)
     {
	    f1.quit();

     }

   }
   if(in.x.ax==3)
      {in.x.ax = 3;
	int86 (0X33,&in,&out);
	x=out.x.cx;
	y=out.x.dx;
		if (x>=20&&x<=50&&y>=7&&y<=25)
		   { hidemouse();
		     bar(20,25,50,50);
		     bar(20,50,300,70);
		     outtextxy(30,53,"Lets you create a new text file");
		     delay(1000);
		     f1.cat();
		   }

	   else if (x>=80&&x<=115&&y>=7&&y<=25)
		   { hidemouse();
		     bar(80,25,115,50);
		     bar(80,50,500,70);
		     outtextxy(90,53,"Lets you open existing text files and edit them");
		     delay(1000);
		     f1.cat();
		   }

	   else if (x>=145&&x<=180&&y>=7&&y<=25)
	   { hidemouse();
	     bar(145,25,180,50);
	     bar(145,50,400,70);
	     outtextxy(150,53,"Guides you how to use the editor");
	     delay(1000);
	     f1.cat();
	   }

	   else if (x>=210&&x<=245&&y>=7&&y<=25)
	   { hidemouse();
	     bar(210,25,245,50);
	     bar(210,50,500,70);
	     outtextxy(215,53,"Exits you from the editor ECLIPSE");
	     delay(1000);
	     f1.cat();
	   }
	}




   }
  }


//////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////


void file::quit()
{hidemouse();
 cleardevice();
 setbkcolor(0);
 setcolor(10);
 outtextxy(30,30,"PLEASE WAIT");

  setcolor(3);
  outtextxy(50,50,"terminating all processes");
  delay(500);
  outtextxy(300,50,".");
  delay(500);
  outtextxy(310,50,".");
  delay(500);
  outtextxy(320,50,".");

  setcolor(14);
  delay(400);
  outtextxy(400,50,"[DONE]");
  delay(200);
  setcolor(3);
  outtextxy(50,80,"Sending all processes KILL signal");
  delay(500);
  outtextxy(350,80,".");
  delay(500);
  outtextxy(360,80,".");
  delay(500);
  outtextxy(370,80,".");
  delay(400);
  setcolor(14);
  outtextxy(400,80,"[DONE]");

  delay(120);
  exit(0);

}
//////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////

void file::saveas()
{hidemouse();
 cleardevice();
 infile.open(fname);
 outtextxy(30,10,"new file name?");
 gets(fname2);
 if(fp)
 {
  cleardevice();
  outtextxy(30,10," unable to create file");
  outtextxy(30,30," file already exists or file creation error");
  getch();
  infile.close();
  f1.cat();
 }
 outfile.open(fname2);
 if(outfile.fail())
 {
  cleardevice();
  outtextxy(30,10," unable to create a file");
  getch();
  infile.close();
  outfile.close();
  f1.cat();
 }

 while(!infile.eof())
 {
  ch=(char)infile.get();
  outfile.put(ch);
 }
  cleardevice();
 outtextxy(30,10,"File saved succesfully");
 infile.close();
 outfile.close();

 getch();
 f1.cat();
}
/////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////

void file::delt()
{hidemouse();
 cleardevice();
 outtextxy(30,10,"Are you sure you want to delete the file");
 gotoxy(5,3);
 cin>>ch;
 if (ch=='y'||ch=='Y')
 { infile.open(fname);
 if (remove(fname)==0)
 cleardevice();
 outtextxy(30,10,"file has been deleted");
 getch();
 f1.cat();}
 else if(ch=='n'||ch=='N')
 f1.cat();
}

/////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////
void file::open_ro()
{ detectmouse();
  showmouse();
  cleardevice();
  setcolor(15);
  rectangle(2,30,637,470);
  rectangle(0,27,637,0);
  bar(12,7,68,25);
  bar(82,7,135,25);
  bar(142,7,200,25);
  bar(212,7,260,25);
  bar(282,7,360,25);
  setcolor(0);
  outtextxy(15,10,"SAVE AS");
  outtextxy(85,10,"RENAME");
  outtextxy(145,10,"DELETE");
  outtextxy(225,10,"EDIT");
  outtextxy(285,10,"MAIN MENU");
  gotoxy(5,3);
  infile.open(fname);
  while (!infile.eof())
  {
   infile.getline(line,max,'@');
	cout<<line;
	infile.close();
  }

  while ( !kbhit () )
  {
   in.x.ax = 3;
   int86 (0X33,&in,&out);

    if(out.x.bx == 1)
    {
    x = out.x.cx;
    y = out.x.dx;
    setcolor(15);
    if(x>=12&&x<=68&&y>=7&&y<=25)
    {     f1.saveas();
    }

    if(x>=82&&x<=135&&y>=7&&y<=25)
    {
	  f1.renam();
    }

    if(x>=142&&x<=200&&y>=7&&y<=25)
    {
	 f1.delt();
    }

    if(x>=212&&x<=260&&y>=7&&y<=25)
    {
	  cleardevice();
	  cleardevice();
	  outtextxy(30,10,"sorry.... can't edit");
	  getch();
	  infile.close();
	  outfile.close();
	  f1.open_ro();
    }

    if(x>=282&&x<=360&&y>=7&&y<=25)
    {   infile.close();
	f1.cat();
    }

   }
  }
}


////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////

void file::open()
{ hidemouse();
  cleardevice();
  outtextxy(30,10,"enter the name of the file to be opened");
  gotoxy(5,3);
  gets(fname);
  infile.open(fname);
  if(infile.fail())
  {
  cleardevice();
  outtextxy(30,10,"no such file exists");
  getch();
  f1.cat();
  }
  else
  {
  detectmouse();
  showmouse();

  cleardevice();
  setcolor(15);
  rectangle(2,30,637,470);
  rectangle(0,27,637,0);
  setcolor(1);
  outtextxy(22,10,"Select option to open the file");
  setcolor(15);
  bar(30,35,100,50);
  bar(150,35,210,50);
  setcolor(1);
  outtextxy(32,37,"read only");
  outtextxy(160,37,"modify");

  while(!kbhit())
  {
   in.x.ax = 3;
   int86 (0X33,&in,&out);
   if(out.x.bx==1)
   {
    x = out.x.cx;
    y = out.x.dx;setcolor(15);
    if(x>=30&&x<=100&&y>=35&&y<=50)
    {infile.close();
     f1.open_ro();
    }

    else if(x>=150&&x<=210&&y>=35&&y<=50)
    {infile.close();
     f1.open_ed();
    }
   }
  }
 }
}
/////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////

void file::open_ed()
{  detectmouse();
 showmouse();

 cleardevice();
  setcolor(15);
  rectangle(2,30,637,470);
  rectangle(0,27,637,0);
  bar(12,7,68,25);
  bar(82,7,135,25);
  bar(142,7,200,25);
  bar(212,7,260,25);
  bar(282,7,360,25);
  setcolor(0);
  outtextxy(15,10,"SAVE AS");
  outtextxy(85,10,"RENAME");
  outtextxy(145,10,"DELETE");
  outtextxy(225,10,"EDIT");
  outtextxy(285,10,"MAIN MENU");
  gotoxy(5,3);
  infile.open(fname);
  while (!infile.eof())
  {
   infile.getline(line,max,'@');
	cout<<line;
	infile.close();
  }

  while ( !kbhit () )
  {
   in.x.ax = 3;
   int86 (0X33,&in,&out);

    if(out.x.bx == 1)
    {
    x = out.x.cx;
    y = out.x.dx;
    setcolor(15);
    if(x>=12&&x<=68&&y>=7&&y<=25)
    {     f1.saveas();
    }

    if(x>=82&&x<=135&&y>=7&&y<=25)
    {
	  f1.renam();
    }

    if(x>=142&&x<=200&&y>=7&&y<=25)
    {
	 f1.delt();
    }

    if(x>=212&&x<=260&&y>=7&&y<=25)
    {  hidemouse();
    cleardevice();
	infile.close();
       infile.open(fname,ios::app);
   outtextxy(10,30,"The existing contents of the file are : ");
   gotoxy(6,13);

   while(!infile.eof())
   {
    infile.getline(line,max,'@');
    cout<<line;
   }
   infile.close();
   cleardevice();
   outtextxy(10,30,"Add watever want to add to existing contents : ");
   outtextxy(10,50,"Don't forget to terminate with @ symbol");
   gotoxy(10,25);


   outfile.open(fname,ios::app);
   cin.getline(line,max,'@');
   outfile<<" "<<line;
   outfile.close();
   f1.cat();
  }
    }

    if(x>=282&&x<=360&&y>=7&&y<=25)
    {   hidemouse();
	infile.close();
	outfile.close();
	f1.cat();
    }

   }
  }



////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////
void file::help()
{ hidemouse();
 cleardevice();
 settextstyle(1,0,5);
 rectangle(2,0,635,470);
 rectangle(0,2,637,468);
 outtextxy(180,10,"ABOUT EDITOR");
 delay(150);
 settextstyle(2,0,5);
 outtextxy(100,160,"This is a text editor created on the basis of C++ programming");
 outtextxy(100,180,"which offers user multiple file options like:");
 outtextxy(100,200,"NEW");
 outtextxy(100,220,"OPEN");
 outtextxy(100,240,"HELP   etc");
 outtextxy(100,260,"to create text files.");
 outtextxy(100,280,"Press any key to proceed and explore");
 getch();
 f1.cat();
 }


////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////

void file::renam()
{hidemouse();
 cleardevice();
 infile.open(fname);
 outfile.open(fname);
 outtextxy(30,10,"enter new file name");
 gets(newname);
 if (rename(fname,newname)==0)
 {
  cleardevice();
  outtextxy(30,10,"Renamed successfully");
  infile.close();
  outfile.close();
  f1.cat();
 }
 else
 {
  cleardevice();
  perror("rename");
  infile.close();
  outfile.close();
  f1.cat();
 }
}
////////////////////////////////////////
// END OF MEMBER FUNCTION DEFINITIONS //
////////////////////////////////////////

/////////////////////////////////////////
////////// MAIN FUNCTION ////////////////
/////////////////////////////////////////

void main()
{

/////////////////////////////////////////
//////////// START GRAPHICS /////////////
/////////////////////////////////////////
 /* request auto detection */
 int gdriver = DETECT, gmode;
 /* initialize graphics and local variables */
 initgraph(&gdriver, &gmode, "\\tc\\bgi");

//////////////////////////////////////////////
///////////////  FIRST PAGE   ////////////////
//////////////////////////////////////////////

setbkcolor(0);

/* draw the boundary using circle */
for(int i=10;i<=625;i++)
circle(i, 10, 5);
for(i=10;i<=625;i++)
circle(i, 470, 5);


for(int j=10;j<=470;j++)
circle(10, j, 5);
for(j=10;j<=470;j++)
circle(625, j, 5);


////////////////////////////////////////
/////////// MAIN TITLE /////////////////
////////////////////////////////////////

 setcolor(14);

 settextstyle(5,0,3);
 outtextxy(200,30,"WELCOME TO THE");
 settextstyle(5,0,6);
 outtextxy(200,40,"EDITOR");
 setcolor(3);
 settextstyle(10,0,5);
 outtextxy(180,100,"E");
 delay(200);
 settextstyle(10,0,5);
 outtextxy(220,100,"C");
 delay(200);
 settextstyle(10,0,5);
 outtextxy(260,100,"L");
 delay(200);
 settextstyle(10,0,5);
 outtextxy(300,100,"I");
 delay(200);
 settextstyle(10,0,5);
 outtextxy(320,100,"P");
 delay(200);
 settextstyle(10,0,5);
 outtextxy(360,100,"S");
 delay(200);
 settextstyle(10,0,5);
 outtextxy(400,100,"E");
delay(500);

settextstyle(3,0,3);
outtextxy(250,390,"CREATED BY:");
setcolor(6);
settextstyle(7,0,3);
outtextxy(50,430,"Ankit Luthra");
outtextxy(240,430,"Aman Kalyan");
outtextxy(450,430,"Akshay Dhar");


 int k=0;
settextstyle(0,0,0);
setcolor(15);
outtextxy(180,308,"NOW LOADING");
outtextxy(420,308,"%");

for(i=280;i<=380;i++)
{
setcolor(4);
circle(i,308, 5);
gotoxy(50,20);
cout<<k++;
delay(20);
}
delay(3000);
settextstyle(0,0,0);

//////////////////////////////////////////
/////////////  NEW  PAGE  ////////////////
//////////////////////////////////////////

cleardevice();
f1.help();
getch();
}

