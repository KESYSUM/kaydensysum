/*
 * Kayden Sysum
 * CS 210
 * 2/8/2025
 * Banking.h
 */

#pragma once
#ifndef BANKING_H
#define BANKING_H

class Banking {
public:
    void SetInitialDeposit(double initialDeposit);
    void SetMonthlyDeposit(double monthlyDeposit);
    void SetAnnualInterestRate(double annualRate);
    void SetInvestmentDuration(int years);

    double GetInitialDeposit() const;
    double GetMonthlyDeposit() const;
    double GetAnnualInterestRate() const;
    int GetInvestmentDuration() const;

    double ComputeBalanceWithoutMonthlyDeposit();
    double ComputeBalanceWithMonthlyDeposit();

private:
    double totalBalance;
    double depositPerMonth;
    double yearlyInterestRate;
    int investmentYears;
};

#endif





/*
 * Kayden Sysum
 * CS 210
 * 2/8/2025
 * BankingMain.cpp
 */

#include <iostream>
#include <iomanip>
#include <string>
#include <cstdlib>
#include <stdexcept>
#include "Banking.h"

using namespace std;

void DisplayMenu(Banking& userInvestment);
int ValidateIntInput();
double ValidateDoubleInput();

int main() {
    Banking userInvestment;
    char userChoice = 'a';
    while (userChoice != 'q') {
        system("cls");
        DisplayMenu(userInvestment);

        userInvestment.ComputeBalanceWithoutMonthlyDeposit();
        userInvestment.ComputeBalanceWithMonthlyDeposit();

        cout << "Enter 'q' to quit or any other key to run another report: ";
        cin >> userChoice;
    }
    return 0;
}

void DisplayMenu(Banking& userInvestment) {
    try {
        double initialDeposit, monthlyDeposit, annualRate;
        int years;

        cout << "**********************************" << endl;
        cout << "********** Data Input ************" << endl;
        cout << "Initial Investment Amount: $";
        initialDeposit = ValidateDoubleInput();
        if (initialDeposit < 0) throw runtime_error("Invalid entry.");
        userInvestment.SetInitialDeposit(initialDeposit);

        cout << "Monthly Deposit: $";
        monthlyDeposit = ValidateDoubleInput();
        if (monthlyDeposit < 0) throw runtime_error("Invalid entry.");
        userInvestment.SetMonthlyDeposit(monthlyDeposit);

        cout << "Annual Interest Rate (%): ";
        annualRate = ValidateDoubleInput();
        if (annualRate < 0) throw runtime_error("Invalid entry.");
        userInvestment.SetAnnualInterestRate(annualRate);

        cout << "Number of Years: ";
        years = ValidateIntInput();
        if (years <= 0) throw runtime_error("Invalid entry.");
        userInvestment.SetInvestmentDuration(years);

        system("PAUSE");
    }
    catch (runtime_error& error) {
        cout << error.what() << endl;
        cout << "Cannot process negative values.\n";
        system("PAUSE");
        system("cls");
        DisplayMenu(userInvestment);
    }
}

int ValidateIntInput() {
    int value;
    while (!(cin >> value)) {
        cout << "Invalid input! Please enter a numerical value: ";
        cin.clear();
        while (cin.get() != '\n');
    }
    return value;
}

double ValidateDoubleInput() {
    double value;
    while (!(cin >> value)) {
        cout << "Invalid input! Please enter a valid number: ";
        cin.clear();
        while (cin.get() != '\n');
    }
    return value;
}













/*
 * Kayden Sysum
 * CS 210
 * 2/8/2025
 * Banking.cpp
 */

#include "Banking.h"
#include <iostream>
#include <iomanip>

using namespace std;

// Banking class definition

// Setters (Mutators)
void Banking::SetInitialDeposit(double initialDeposit) {
    totalBalance = initialDeposit;
}
void Banking::SetMonthlyDeposit(double monthlyDeposit) {
    depositPerMonth = monthlyDeposit;
}
void Banking::SetAnnualInterestRate(double annualRate) {
    yearlyInterestRate = annualRate;
}
void Banking::SetInvestmentDuration(int years) {
    investmentYears = years;
}

// Getters (Accessors)
double Banking::GetInitialDeposit() const {
    return totalBalance;
}
double Banking::GetMonthlyDeposit() const {
    return depositPerMonth;
}
double Banking::GetAnnualInterestRate() const {
    return yearlyInterestRate;
}
int Banking::GetInvestmentDuration() const {
    return investmentYears;
}

// Function to calculate and display balance without monthly deposits
double Banking::ComputeBalanceWithoutMonthlyDeposit() {
    double currentBalance = totalBalance;

    cout << "\n     Balance and Interest Without Monthly Deposits" << endl;
    cout << string(70, '=') << endl;
    cout << "Year          End Balance          Earned Interest" << endl;
    cout << string(70, '-') << endl;

    for (int i = 0; i < investmentYears; i++) {
        double interestAmount = currentBalance * (yearlyInterestRate / 100);
        currentBalance += interestAmount;
        cout << " " << left << setw(5) << (i + 1) << "\t\t$" << fixed << setprecision(2) << currentBalance
            << "\t\t\t$" << interestAmount << endl;
    }
    return currentBalance;
}

// Function to calculate and display balance with monthly deposits
double Banking::ComputeBalanceWithMonthlyDeposit() {
    double currentBalance = totalBalance;

    cout << "\n     Balance and Interest With Monthly Deposits" << endl;
    cout << string(70, '=') << endl;
    cout << "Year          End Balance          Earned Interest" << endl;
    cout << string(70, '-') << endl;

    for (int i = 0; i < investmentYears; i++) {
        double yearlyTotalInterest = 0;
        for (int j = 0; j < 12; j++) {
            double interestAmount = (currentBalance + depositPerMonth) * ((yearlyInterestRate / 100) / 12);
            yearlyTotalInterest += interestAmount;
            currentBalance += depositPerMonth + interestAmount;
        }
        cout << " " << left << setw(5) << (i + 1) << "\t\t$" << fixed << setprecision(2) << currentBalance
            << "\t\t\t$" << yearlyTotalInterest << endl;
    }
    return currentBalance;
}
