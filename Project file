#include <iostream>
#include <fstream>
#include <iomanip>
#include <vector>
#include <string> 
using namespace std;

struct townInfo
{
  string TOWN,
         REG;
         
  int ID,
      MOL,
      SER,
      POP,
      LUP;

  bool readable = true;
};

//Prototypes
void showTownInfo(townInfo pTown);
void splitAndFill(string pStringTown, townInfo& pTown, char delim);
void showChart(townInfo ptownInfo);
void print_file(FILE *fp);

int main() 
{
  int option;
  bool done = false;

  //Variables
  townInfo Pueblo;
  
  //Vector
  vector<townInfo> arrDataFrame;

  cout << "\nWelcome to the CoRE COVID-19 Dashboard and Analyzer®\n";
  cout << "Please choose one of the following options!\n";
  cout << "Recommended to first choose option 9 to load the information that is already stored.\n";  
  
  while(!done)
  {
   cout << "---------------------------------------------------\n";
   cout << "1. Display Data Frame\n";
   cout << "2. Display a town\n";
   cout << "3. Add a town\n";
   cout << "4. Edit a town observation\n";
   cout << "5. Delete a town\n";
   cout << "6. Find towns by region\n";
   cout << "7. Find observation by town name\n";
   cout << "8. Save observations to file\n";
   cout << "9. Load observations from file\n";
   cout << "10. Create Bar chart\n";
   cout << "11. Exit application\n";
   cout << "---------------------------------------------------\n";
   cout << "Enter option number: ";
   cin >> option;
  
   switch(option)
   {
    case 1: //Display Data Frame
    {
      ifstream dataFrameFile;
      dataFrameFile.open("PuertoRicoCovid19DF.csv");
      string data;

      cout << "\nID  TOWN       REGION      MOLECULAR  SEROLOGIC POPULATION   L. UPDATE\n";
      cout << "--- ---------  ----------- ---------- --------- ----------   ----------\n";
      for(int i=0; i < arrDataFrame.size(); i++)
      {
        showChart(arrDataFrame[i]); //Display info loaded from file
      }
      cout << "--- ---------  ----------- ---------- --------- ----------   ----------\n";
     
      dataFrameFile.close();
      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get();
      continue;      
    }

    case 2: //Display a Town
    {
      int id;
 
      cout << "\nDISPLAYING A TOWN\n";
      cout << "---------------------------------------------------\n";
      cout << "Enter the observation id: ";
      cin >> id;
      cout << "---------------------------------------------------\n"; 
      if(id < 79 || id > 0 || !arrDataFrame.empty())
      {
        showTownInfo(arrDataFrame[id-1]); // Display chosen town info
      }else if(id > arrDataFrame.size() || id < arrDataFrame.size() || arrDataFrame.empty())     
      {
        cout << "This observation is not available!\n";
      }


      cout << "---------------------------------------------------\n";
      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get();
      continue;
    }

    case 3: //add town
    {
      cout << "\nPlease input the new info\n";
      cout << "---------------------------------------------------\n";
      cout << "ID: ";
      cin >> Pueblo.ID; //Add ID

      cout << "Town: ";
      cin >> Pueblo.TOWN; //Add Townname

      cout << "Region: "; //Add Town Region
      cin >> Pueblo.REG;

      cout << "Molecular: "; //Add number of molecular cases
      cin >> Pueblo.MOL;

      cout << "Serologic: "; //Add number of serological cases
      cin >> Pueblo.SER;

      cout << "Population: "; //Add population
      cin >> Pueblo.POP;
        
      cout << "L.Update(mmddYYYY): "; //Add date of Last Update
      cin >> Pueblo.LUP;

      arrDataFrame.push_back(Pueblo); //Add to vector
      cout << "---------------------------------------------------\n";
      cout << "The information has beed added!\n";
      cout << "---------------------------------------------------\n";

      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get();
      continue;        
    } 

    case 4: //Edit Town
    {
      int obID;
      cout << "EDITING A TOWN OBSERVATION\n";
      cout << "---------------------------------------------------\n";
      cout << "Enter the observation ID: ";
      cin >> obID;
      if(obID < 79 && obID > 0)
      {
        cout << "\nCURRENT Information: \n";
        cout << "---------------------------------------------------\n";
        showTownInfo(arrDataFrame[obID-1]);
        cout << "---------------------------------------------------\n";
        cout << "Please enter the new information for the town: \n";
        cout << "Molecular: ";
        cin >> arrDataFrame[obID-1].MOL; //Edit molecular cases
        cout << "Serologic: ";
        cin >> arrDataFrame[obID-1].SER; //Edit serological cases
        cout << "Population: ";
        cin >> arrDataFrame[obID-1].POP; //Edit population
        cout << "L. Update(mm:dd:YYYY): ";
        cin >> arrDataFrame[obID-1].LUP; //Edit date of Last Update
        cout << "---------------------------------------------------\n";
      }else
      {
        cout << "---------------------------------------------------\n";
        cout << "Did not find any observation with that id!\n";
        cout << "---------------------------------------------------\n";
      }
      cout << "<Press Enter to continue>\n";
      cin.ignore();
      cin.get(); 
      continue;
    }  
    
    case 5: //Delete town
    {
      char filename[100] = "PuertoRicoCovid19DF.csv",
           str[100];

      FILE * fp1, *fp2;

      int del_line_no,
          line_no = 0;

      fp1 = fopen(filename, "r");
      cout << "\nContent of file before modification : \n";
      cout << "---------------------------------------------------\n";
      print_file(fp1);
      rewind(fp1);

      cout << "---------------------------------------------------\n";

      cout << "Enter the line number to be deleted : ";
      cin >> del_line_no;
        
      //deleting observation in file
      fp2 = fopen("replica.txt", "w");

      while( fgets(str, 99, fp1) != NULL)
      {
        line_no++;

        if(line_no != del_line_no)
        {
          fputs(str, fp2); //Places info in different file
        }
      }

      fclose(fp1);
      fclose(fp2);

      remove(filename); //Deletes original file
      rename("replica.txt", filename); //Renamos new file as the old file

      fp1 = fopen(filename, "r"); //Open new file

      printf("\nContent of file after modification : \n"); //Display new file
      cout << "---------------------------------------------------\n";
      print_file(fp1);
      fclose(fp1);

      cout << "---------------------------------------------------\n";
      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get(); 
      continue;
    }
    
    case 6: //Find town by region
    {
      string search,
             line;
      int offset;

      ifstream MyOtherfile;
      MyOtherfile.open ("PuertoRicoCovid19DF.csv");
      cout << "Type the region you're looking for\n";
      cin >> search;

      if (MyOtherfile.is_open())
      {
        cout << "Towns in the " << search << " region:\n";
        cout << "---------------------------------------------------\n";
        while (!MyOtherfile.eof())
        {
          getline(MyOtherfile, line);
          if ((offset = line.find(search)) != string::npos)
          {
            cout << line << endl; //Displpay towns with chosen region

          }else if((offset = line.find(search)) != string::npos)
          {
            cout << "Could not find the town. Please try again." << endl;
            break;
          }
        }
          MyOtherfile.close();
      }
      cout << "---------------------------------------------------\n";
      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get(); 
      continue;
    }
     
    case 7: //Find observation by town name
    {
      string search,
             line;
      int offset;

      ifstream MyOtherfile;
      MyOtherfile.open ("PuertoRicoCovid19DF.csv");
      cout << "Type the town you're looking for\n";
      cin >> search;

      if (MyOtherfile.is_open())
      {
        cout << "Towns named " << search << ":\n";
        cout << "---------------------------------------------------\n";
        while (!MyOtherfile.eof())
        {
          getline(MyOtherfile, line);
          if ((offset = line.find(search)) != string::npos)
          {
            cout << line << endl; //Display towns that were called

          }
            else if((offset = line.find(search)) != string::npos)
            {
              cout << "Could not find the town. Please try again." << endl;
              break;
            }
        }
          MyOtherfile.close();
      }
      cout << "---------------------------------------------------\n";
      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get(); 
      continue;
    }
    
    case 8: //Save
    {
      //open up the file to store things inside
      ofstream fileTownInfoOut;
      fileTownInfoOut.open("PuertoRicoCovid19DF.csv");
            
      //put the vector into the file
      if(!fileTownInfoOut)
      {
        cout << "Could not save to file!\n";

      }else 
      {
        for (int i = 0; i < arrDataFrame.size(); i++) //Saves all info inside vectors into
        {
          fileTownInfoOut << arrDataFrame[i].ID << "|";
          fileTownInfoOut << arrDataFrame[i].TOWN << "|" << arrDataFrame[i].REG << "|"; 
          fileTownInfoOut << arrDataFrame[i].MOL << "|" << arrDataFrame[i].SER << "|";
          fileTownInfoOut << arrDataFrame[i].POP << "|" << arrDataFrame[i].LUP << endl;
          Pueblo.readable = true;
        }
        //close the file
        fileTownInfoOut.close();
        cout << "Town Observations have been saved!\n";        
      }
    
      cout << "---------------------------------------------------\n";
      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get();
      continue;
    }

    case 9: //Load
    {
      string loadLine;
      ifstream inLoad;

      inLoad.open("PuertoRicoCovid19DF.csv");

      if(!inLoad)
      {
        cout <<"\nError loading info!";
        cout << "---------------------------------------------------\n";
        cout << "\n<Press Enter to continue>\n";
        cin.ignore();
        cin.get(); 
        continue;
      }

      while(getline(inLoad, loadLine))
      {
        splitAndFill(loadLine, Pueblo, '|');
        arrDataFrame.push_back(Pueblo);
      }
    
      inLoad.close();

      cout << "\nInfo has been loaded!\n";
      cout << "---------------------------------------------------\n";
      cout << "\n<Press Enter to continue>\n";
      cin.ignore();
      cin.get(); 
      continue;  
    }
    
    case 10: //Bar chart
    {
      char testOption,
           molBar,
           serBar;
      
      cout << "\nWelcome to the CoRE COVID-19 Dashboard and Analyzer® Bar Chart\n";
      cout << "What type of tests would you like to see?:\n\n";
      cout << "A. Molecular Tests.\n";
      cout << "B. Seroligical Tests\n";
      cout << "Enter Option here: ";
      cin >> testOption;
      cout << "---------------------------------------------------\n";

      switch(testOption)
      {
        case 'A': //Shows Bar Chart for Molecular cases
        {
          cout << "Molecular COVID-19 Cases.\n";
          cout << "Each * equals to 5 cases.\n\n";
          for(int i=0; i < 78; i++)
          {
            if(arrDataFrame[i].ID < 1)
            {
              cout << "";
            }else
            {
              molBar = arrDataFrame[i].MOL / 5;
              cout << "-" << arrDataFrame[i].TOWN << ": ";
              for(int f=0; f < molBar; f++)
              {
                cout << "*";
              }
              cout << "|" << arrDataFrame[i].MOL << endl;
            }
          }
        }
        cout << "---------------------------------------------------\n";
        cout << "\n<Press Enter to continue>\n";
        cin.ignore();
        cin.get(); 
        continue;   

        case 'B': //Shows Bar Chart for Serological cases
        {
          cout << "Serological COVID-19 Cases.\n";
          cout << "Each * equals to 5 cases.\n\n";
          for(int i=0; i < 78; i++)
          {
            if(arrDataFrame[i].ID < 1)
            {
              cout << "";
            }else
            {
              serBar = arrDataFrame[i].SER / 5;
              cout << "-" << arrDataFrame[i].TOWN << ": ";
              for(int f=0; f < serBar; f++)
              {
                cout << "*";
              }
              cout << "|" << arrDataFrame[i].SER << endl;
            }
          }
        }
        cout << "---------------------------------------------------\n";
        cout << "\n<Press Enter to continue>\n";
        cin.ignore();
        cin.get(); 
        continue; 

        default:
        {
          cout << "\nThe entry was incorrect!\n";
          cout << "Please choose the correct option!";
          continue;
        }

      } 
    }
    
    case 11: //Exit
    {
      cout <<"Thanks for using the CoRE COVID-19 Dashboard and Analyzer®";
      done = true;
      continue;
    }

    default:
    {
      cout << "\nThe entry was incorrect!\n";
      cout << "Please choose the correct option.\n";
      continue;
    }
   }
  }
  return 0;
}

void showTownInfo(townInfo ptownInfo)
{
  cout << "Town information: \n";
  cout << "ID" << setw(13) << ": " << ptownInfo.ID << endl;
  cout << "Town" << setw(11) << ": " << ptownInfo.TOWN << endl;
  cout << "Region" << setw(9) << ": " << ptownInfo.REG << endl;
  cout << "Molecular" << setw(6) << ": " << ptownInfo.MOL << endl;
  cout << "Serological" << setw(4) << ": " << ptownInfo.SER << endl;
  cout << "Population" << setw(5) << ": " << ptownInfo.POP << endl;
  cout << "L.Update" << setw(7) << ": " << ptownInfo.LUP << endl;
}

void splitAndFill(string pStringTown, townInfo& pTown, char delim)
{
  string chunkStr; //guarda un chunk del string original
  vector<string> locTown; //guarda el string dividido
  size_t current, previous = 0; //current es la posicion del delim

  //busco la primera aparicion del delim
  current = pStringTown.find(delim);
  while (current != string::npos)
  {
    //selecciono el chunk
    chunkStr = pStringTown.substr(previous, current - previous);
    //inserto el chunk en el vector
    locTown.push_back(chunkStr);
    previous = current + 1;
    current = pStringTown.find(delim, previous);
  }

  //selecciono el ultimo chunk se string
  chunkStr = pStringTown.substr(previous, current - previous);
  //inserto el ultimo chunk al vector local
  locTown.push_back(chunkStr);
  pTown.ID = stoi(locTown[0]);
  pTown.TOWN = locTown[1];
  pTown.REG = locTown[2];
  pTown.MOL = stoi(locTown[3]);
  pTown.SER = stoi(locTown[4]);
  pTown.POP = stoi(locTown[5]);
  pTown.LUP = stoi(locTown[6]);
  pTown.readable = true;
}

void showChart(townInfo ptownInfo)
{ 
    cout << setw(2) << ptownInfo.ID << setw(10) << ptownInfo.TOWN << setw(13) << ptownInfo.REG << setw(11) << ptownInfo.MOL << setw(10);
    cout << ptownInfo.SER << setw(11) << ptownInfo.POP << setw(14) << ptownInfo.LUP << endl;
}
 
void print_file(FILE *fp)
{
    char ch;

    while((ch = fgetc(fp)) != EOF)
    {
        printf("%c", ch);
    }
}
