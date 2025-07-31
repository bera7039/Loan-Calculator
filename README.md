# Loan-Calculator
from collections import deque

class LoanCalculator:
    def __init__(self, principal, annual_rate, years):
        self.principal = principal
        self.annual_rate = annual_rate
        self.years = years
        self.monthly_rate = self.annual_rate / 12 / 100
        self.months = self.years * 12
        self.emi = self.calculate_emi()
        self.payment_history = deque()

    def calculate_emi(self):
        R = self.monthly_rate
        N = self.months
        P = self.principal
        emi = (P * R * (1 + R) ** N) / ((1 + R) ** N - 1)
        return round(emi, 2)

    def generate_amortization_schedule(self):
        balance = self.principal
        schedule = []
        for month in range(1, self.months + 1):
            interest = balance * self.monthly_rate
            principal_paid = self.emi - interest
            balance -= principal_paid
            self.payment_history.append({
                'Month': month,
                'EMI': round(self.emi, 2),
                'Interest': round(interest, 2),
                'Principal': round(principal_paid, 2),
                'Remaining Balance': round(balance, 2)
            })
            schedule.append(self.payment_history[-1])
        return schedule

    def print_summary(self):
        print(f"\nLoan Summary:")
        print(f"Loan Amount: ₹{self.principal}")
        print(f"Annual Interest Rate: {self.annual_rate}%")
        print(f"Duration: {self.years} years")
        print(f"EMI: ₹{self.emi}\n")

    def print_schedule(self):
        print("Month | EMI     | Interest | Principal | Remaining Balance")
        for record in self.payment_history:
            print(f"{record['Month']:>5} | ₹{record['EMI']:>7} | ₹{record['Interest']:>8} | ₹{record['Principal']:>9} | ₹{record['Remaining Balance']:>17}")

# === Run the Calculator ===
if __name__ == "__main__":
    P = float(input("Enter loan amount (₹): "))
    R = float(input("Enter annual interest rate (%): "))
    Y = int(input("Enter loan duration (years): "))

    loan = LoanCalculator(P, R, Y)
    loan.print_summary()
    loan.generate_amortization_schedule()
    loan.print_schedule()
