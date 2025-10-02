# Python
# 📅 Metadata
Date <- Sys.Date()
Shift <- "Night"
Attendant <- "Nelson Wasike"
StationID <- "Luqman Lavington"

# 🔢 Meter Readings
U1E <- 163677.762; U1M <- 164000.535
D1E <- 32042.599;  D1M <- 32044.986
U2E <- 208045.911; U2M <- 208506.588
D2E <- 26119.542;  D2M <- 26163.722

# 💰 Prices per Litre
UP <- 181.80
DP <- 167.50

# 💳 Payment Details
Mpesa <- 107446
Visa <- 19337
Discount <- 0
Expense <- 0
Drops <- 15000
Coins <- 80
LCash <- 8250
Hanging <- 0
Unclaimed <- 0
Invoice <- 0

# 🧮 Litres Sold
U1L <- U1M - U1E
D1L <- D1M - D1E
U2L <- U2M - U2E
D2L <- D2M - D2E
TL <- U1L + D1L + U2L + D2L

# 💵 Money Collected
U1S <- U1L * UP
D1S <- D1L * DP
U2S <- U2L * UP
D2S <- D2L * DP
Expected <- U1S + D1S + U2S + D2S
ExpectedInv <- Expected - Invoice
ActualAmount <- Mpesa + Visa + Discount + Expense + Drops + Coins + LCash + Hanging + Unclaimed
EL <- ActualAmount - ExpectedInv

# 📈 Performance Metrics
RevenuePerLitre <- Expected / TL
VariancePercent <- (ActualAmount - ExpectedInv) / ExpectedInv * 100

# 🚦 Visual Alert
if (abs(VariancePercent) < 1) {
  Alert <- "✅ Variance within acceptable range"
} else if (abs(VariancePercent) < 5) {
  Alert <- "⚠️ Moderate variance — check details"
} else {
  Alert <- "❌ High variance — investigate immediately"
}

# 📊 Summary Table
summary_table <- data.frame(
  Pump = c("U1", "D1", "U2", "D2"),
  Litres_Sold = round(c(U1L, D1L, U2L, D2L), 3),
  Price_per_Litre = c(UP, DP, UP, DP),
  Revenue = round(c(U1S, D1S, U2S, D2S), 2)
)

# 🧾 Financial Summary
financial_summary <- data.frame(
  Item = c("Expected Amount", "Invoice", "Expected - Invoice", "Actual Amount", "Excess / Loss", "Revenue per Litre", "Variance (%)", "Alert"),
  Amount = c(round(Expected, 2), Invoice, round(ExpectedInv, 2), ActualAmount, round(EL, 2), round(RevenuePerLitre, 2), round(VariancePercent, 2), Alert)
)

# 📤 Optional Export to Excel
# Uncomment below if you want to export
# install.packages("openxlsx")  # Run once
# library(openxlsx)
# write.xlsx(list(Summary = summary_table, Financials = financial_summary),
#            file = paste0("Fuel_Report_", Date, "_", Shift, ".xlsx"))

# 📋 Display Results
cat("📅 Fuel Sales Report —", Date, Shift, "\n")
cat("👨‍🔧 Attendant:", Attendant, "| Station:", StationID, "\n\n")
print(summary_table)
cat("\n💰 Financial Summary:\n")
print(financial_summary)