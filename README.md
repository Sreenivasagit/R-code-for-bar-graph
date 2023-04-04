# R-code-for-bar-graph
As I am learning R thought of generate bar graph from 3 different protein expression in 7 samples
# Create the data frame
data <- data.frame(Sox2 = c(23, 34, 35, 23, 56, 78, 54),
                   GFAP = c(89, 79, 68, 56, 48, 69, 89),
                   Olig2 = c(67, 89, 80, 78, 99, 80, 79))

# Set the column names to the category names
colnames(data) <- c("Sox2", "GFAP", "Olig2")

# Calculate the mean, median, and standard deviation for each column
summary <- data.frame(Mean = apply(data, 2, mean),
                      Median = apply(data, 2, median),
                      SD = apply(data, 2, sd))

# Group the samples by category
Sox2_data <- data$Sox2
GFAP_data <- data$GFAP
Olig2_data <- data$Olig2

# Create the barplot
barplot(
  cbind(Sox2_data, GFAP_data, Olig2_data),
  beside = TRUE,
  col = c("red", "green", "blue"),
  ylim = c(0, max(data) * 1.1),
  ylab = "Count",
  names.arg = colnames(data),
  legend.text = c("Sox2", "GFAP", "Olig2"),
  args.legend = list(x = "topright")
)

# Add the error bars
arrows(
  x0 = barplot(cbind(Sox2_data, GFAP_data, Olig2_data), beside = TRUE)[1, ] - 0.15,
  y0 = summary$Mean - summary$SD,
  x1 = barplot(cbind(Sox2_data, GFAP_data, Olig2_data), beside = TRUE)[1, ] + 0.15,
  y1 = summary$Mean + summary$SD,
  length = 0.05,
  angle = 90,
  code = 3
)

# Add the mean and median lines
abline(h = summary$Mean, col = "black", lty = 2)
abline(h = summary$Median, col = "red", lty = 2)

# Add the category names
title(main = "Bar Plot of Sample Counts by Category")

# Add the sample points
points(
  rep(1:ncol(data), each = nrow(data)),
  as.vector(data),
  pch = "."
)

# Adjust the margins
par(mar = c(5, 4, 4, 6) + 0.1)
