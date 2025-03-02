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
