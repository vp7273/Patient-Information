

#include iostream
#include iomanip

using namespace std;

//structure definitions
struct PatientVitalInfo
{
	float temperature;
	unsigned int systolicPressure;
	unsigned int diastolicPressure;
};

struct PatientActivityInfo
{
	unsigned int stepCount;
	unsigned int sleepHours;
};

union InformationType
{

	//v and a are to allow access to nested structures vitals and activity
	PatientVitalInfo v;
	PatientActivityInfo a;
};
enum type { VITALS, ACTIVITY };

struct StoreUnion
{
	InformationType patientinfo;
	type datatype;
};
//structures for later functions to return more than one value (calculated max and minimums and totals)
struct maxMinTemp
{
	float maxTemp;
	float minTemp;
};
struct maxMinSystolic
{
	unsigned int maxSystolic;
	unsigned int minSystolic;
};
struct maxMinDiastolic
{
	unsigned int maxDiastolic;
	unsigned int minDiastolic;
};

struct ActivityTotals
{
	unsigned int totalstep;
	unsigned int totalsleep;

};

//function prototypes
void patientVitals(StoreUnion[], int);
void patientActivity(StoreUnion[], int);
void printVitalInfo(maxMinTemp, maxMinSystolic, maxMinDiastolic, int);
void printActivityInfo(ActivityTotals, int);
maxMinTemp functiontemp(StoreUnion[], int);
maxMinSystolic functionsystolic(StoreUnion[], int);
maxMinDiastolic functiondiastolic(StoreUnion[], int);
ActivityTotals functionactivity(StoreUnion[], int);


int main()
{

	const int ARRAYSIZE = 1000;
	StoreUnion patientarray[ARRAYSIZE];

	int userselection;
	//counts how many times each function called for subscript notation later
	int vitalcount = 0, activitycount = 0, functioncallcount = 0;

	do
	{
		cout << "Please enter the number for the desired action (1, 2, 3):" << endl;
		cout << " 1 - Enter some patient vital information" << endl;
		cout << "	2 - Enter some patient activity information" << endl;
		cout << "	3 - Print summary information on the patient information" << endl;
		cout << "and exit the program" << endl;

		cin >> userselection;

		while (((userselection != 1) && (userselection != 2) && (userselection != 3) )|| (cin.fail()))
		{
			cout << "Please enter 1, 2, or 3" << endl << endl;
			cout << "Please enter the number for the desired action (1, 2, 3):" << endl;
			cout << " 1 - Enter some patient vital information" << endl;
			cout << "	2 - Enter some patient activity information" << endl;
			cout << "	3 - Print summary information on the patient information" << endl;
			cout << "and exit the program" << endl;
			cin.clear();
			cin.ignore(10000, '\n');
			cin >> userselection;
		}

		if (userselection == 1)
		{

			patientVitals(patientarray, functioncallcount);
			vitalcount++;

		}


		if (userselection == 2)
		{
			patientActivity(patientarray, functioncallcount);
			activitycount++;
		}

		if (userselection == 3)
		{

			cout << "Number of patient vital information records:      " << vitalcount << endl;
			if (vitalcount > 0)
			{
				maxMinTemp tempstructure = functiontemp(patientarray, functioncallcount);
				maxMinSystolic systolicstructure = functionsystolic(patientarray, functioncallcount);
				maxMinDiastolic diastolicstructure = functiondiastolic(patientarray, functioncallcount);

				printVitalInfo(tempstructure, systolicstructure, diastolicstructure, vitalcount);
			}
			cout << "Number of patient activity information records:   " << activitycount << endl;
			if (activitycount > 0)
			{
				ActivityTotals activitystructure = functionactivity(patientarray, functioncallcount);
				printActivityInfo(activitystructure, activitycount);
			}



		}


		functioncallcount++;
		cout << endl;

	} while (userselection != 3);




	system("PAUSE");
	return 0;
}

void patientVitals(StoreUnion patientarray[], int subscript)
{

	float temp;
	unsigned int systolic, diastolic;

	cin.clear(); //clearing cin between iterations of the loop to prevent unnecessary error message
	cin.ignore(10000, '\n');
	cout << "Enter the temperature: ";
	cin >> temp;
	while (cin.fail() || temp <0)
	{
		cin.clear();
		cin.ignore(10000, '\n');
		cout << "Please enter a positive float number" << endl;
		cout << "Enter the temperature: ";
		cin >> temp;
	}
	patientarray[subscript].patientinfo.v.temperature = temp;

	cin.clear();
	cin.ignore(10000, '\n');
	cout << "Enter the systolic pressure: ";
	cin >> systolic;
	while ((cin.fail()) || int(systolic) < 0)
	{
		cin.clear();
		cin.ignore(10000, '\n');
		cout << "Please enter an integral unsigned number" << endl;
		cout << "Enter the systolic pressure: ";
		cin >> systolic;
	}
	patientarray[subscript].patientinfo.v.systolicPressure = systolic;

	cin.clear();
	cin.ignore(10000, '\n');
	cout << "Enter the diastolic pressure: ";
	cin >> diastolic;
	while (cin.fail() || (int)diastolic <0)
	{
		cin.clear();
		cin.ignore(10000, '\n');
		cout << "Please enter an integral unsigned number" << endl;
		cout << "Enter the diastolic pressure: ";
		cin >> diastolic;
	}

	patientarray[subscript].patientinfo.v.diastolicPressure = diastolic;
	patientarray[subscript].datatype = VITALS;
}

void patientActivity(StoreUnion patientarray[], int subscript)
{

	unsigned int step, sleep;

	cout << "Enter the step count: ";
	cin.clear();
	cin.ignore(10000, '\n');
	cin >> step;
	while (cin.fail() || (int)step <0)
	{
		cin.clear();
		cin.ignore(10000, '\n');
		cout << "Please enter an integral unsigned number" << endl;
		cout << "Enter the step count: ";
		cin >> step;
	}
	patientarray[subscript].patientinfo.a.stepCount = step;


	cout << "Enter the sleep hours: ";
	cin.clear();
	cin.ignore(10000, '\n');
	cin >> sleep;
	while (cin.fail() || (int)sleep <0)
	{
		cin.clear();
		cin.ignore(10000, '\n');
		cout << "Please enter an integral unsigned number" << endl;

		cout << "Enter the sleep hours: ";
		cin >> sleep;
	}

	patientarray[subscript].patientinfo.a.sleepHours = sleep;
	patientarray[subscript].datatype = ACTIVITY;
}

void printVitalInfo(maxMinTemp tempstructure, maxMinSystolic systolicstructure, maxMinDiastolic diastolicstructure, int vitalcount)
{


	//vitalcount is # of times patientvitals function called
	//if statements to only print if user inputted that type info at all



	if (vitalcount > 0)
	{
		cout << setw(50)<< left<< " Maximum temperature:" << setprecision(1) << fixed << tempstructure.maxTemp << endl;
		cout << setw(50)<< left<<" Minimum temperature:" << setprecision(1) << fixed  << tempstructure.minTemp << endl;
		cout << setw(50) << left<<" Maximum systolic pressure:"  << systolicstructure.maxSystolic << endl;
		cout << setw(50) << left<<" Minimum systolic pressure:"  << systolicstructure.minSystolic << endl;
		cout << setw(50) << left << " Maximum diastolic pressure:"  << diastolicstructure.maxDiastolic << endl;
		cout << setw(50) << left << " Minimum diastolic pressure:" << diastolicstructure.minDiastolic << endl;
	}


}

void printActivityInfo(ActivityTotals activitystructure, int activitycount)
{


	//activitycount is # of times patientvitals function called
	//if statements to only print if user inputted that type info at all

	if (activitycount > 0)
	{
		cout << setw(50) << left << " Total step count:"  << activitystructure.totalstep << endl;
		cout << setw(50) << left << " Total sleep hours:"  << activitystructure.totalsleep << endl;
	}

}
//functioncallcount is passed to all three max/min functions to keep track of how
//many times user inputted information/how much array is holding/how many times to run loop

maxMinTemp functiontemp(StoreUnion patientarray[], int functioncallcount)
{


	maxMinTemp localTempStructure;

	float mintemp = INT16_MAX, maxtemp = INT16_MIN;
	//both initialized to first element
	for (int loopVar = 0; loopVar < functioncallcount; loopVar++)
	{
		if (patientarray[loopVar].datatype == VITALS)
		{
			mintemp = patientarray[loopVar].patientinfo.v.temperature;
			maxtemp = patientarray[loopVar].patientinfo.v.temperature;
			break;
		}

		else
			break;
	}

	for (int index = 0; index < functioncallcount; index++)
	{
		if (patientarray[index].datatype == VITALS)
		{
			if (patientarray[index].patientinfo.v.temperature < mintemp)
				mintemp = patientarray[index].patientinfo.v.temperature;

			if (patientarray[index].patientinfo.v.temperature > maxtemp)
				maxtemp = patientarray[index].patientinfo.v.temperature;
		}

	}

	localTempStructure.maxTemp = maxtemp;
	localTempStructure.minTemp = mintemp;

	return localTempStructure;

}
maxMinSystolic functionsystolic(StoreUnion patientarray[], int functioncallcount)
{


	maxMinSystolic localSystolicStructure;

	unsigned int minsystolic = INT16_MAX, maxsystolic = INT16_MIN;
	//both initialized to first element
	for (int loopVar = 0; loopVar < functioncallcount; loopVar++)
	{
		if (patientarray[loopVar].datatype == VITALS)
		{
			minsystolic = patientarray[loopVar].patientinfo.v.systolicPressure;
			maxsystolic = patientarray[loopVar].patientinfo.v.systolicPressure;
			break;
		}
		else
			break;
	}



	for (int index = 0; index < functioncallcount; index++)
	{
		if (patientarray[index].datatype == VITALS)
		{
			if (patientarray[index].patientinfo.v.systolicPressure < minsystolic)
				minsystolic = patientarray[index].patientinfo.v.systolicPressure;

			if (patientarray[index].patientinfo.v.systolicPressure > maxsystolic)
				maxsystolic = patientarray[index].patientinfo.v.systolicPressure;
		}
	}

	localSystolicStructure.maxSystolic = maxsystolic;
	localSystolicStructure.minSystolic = minsystolic;

	return localSystolicStructure;

}
maxMinDiastolic functiondiastolic(StoreUnion patientarray[], int functioncallcount)
{


	maxMinDiastolic localDiastolicStructure;

	unsigned int mindiastolic = INT16_MAX, maxdiastolic = INT16_MIN;
	//both initialized to first element
	for (int loopVar = 0; loopVar < functioncallcount; loopVar++)
	{
		if (patientarray[loopVar].datatype == VITALS)
		{			mindiastolic = patientarray[loopVar].patientinfo.v.diastolicPressure;
			maxdiastolic = patientarray[loopVar].patientinfo.v.diastolicPressure;
			break;
		}
		else
			break;
	}


	for (int index = 1; index < functioncallcount; index++)
	{
		if (patientarray[index].datatype == VITALS)
		{
			if (patientarray[index].patientinfo.v.diastolicPressure < mindiastolic)
				mindiastolic = patientarray[index].patientinfo.v.diastolicPressure;

			if (patientarray[index].patientinfo.v.diastolicPressure > maxdiastolic)
				maxdiastolic = patientarray[index].patientinfo.v.diastolicPressure;
		}

	}

	localDiastolicStructure.maxDiastolic = maxdiastolic;
	localDiastolicStructure.minDiastolic = mindiastolic;

	return localDiastolicStructure;
}

ActivityTotals functionactivity(StoreUnion patientarray[], int functioncallcount)
{

	ActivityTotals localActivityStructure;

	unsigned int stepSum = 0, sleepSum = 0;

	for (int index = 0; index < functioncallcount; index++)
	{
		if (patientarray[index].datatype == ACTIVITY)
		{
			stepSum += patientarray[index].patientinfo.a.stepCount;
			sleepSum += patientarray[index].patientinfo.a.sleepHours;
		}
	}

	localActivityStructure.totalstep = stepSum;
	localActivityStructure.totalsleep = sleepSum;

	return localActivityStructure;
}





 # Patient-Information
This program utilizes mutliple ADTs to store user-entered information about patient health. 
