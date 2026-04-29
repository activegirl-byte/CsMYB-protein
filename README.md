# CsMYB-protein
# 加载包
library(ggplot2)
library(reshape2)
setwd("E:\\Cs_MYB")
# ---------------------- 1. 读入数据 ----------------------
# 加载包
library(ggplot2)
library(reshape2)

# ===================== 从 data.txt 读取数据 =====================
# 你的文件是制表符分隔，带中文表头，直接读取
data <- read.delim("data.txt", sep = "\t", stringsAsFactors = FALSE, check.names = FALSE)

# 重命名列名（方便后续处理）
colnames(data) <- c("GeneID", "Alpha_Helix", "Beta_Turn", "Extended_strand", "Random_coil")

# 转换为长格式（ggplot 绘图必须）
data_long <- melt(
  data,
  id.vars = "GeneID",
  measure.vars = c("Alpha_Helix", "Beta_Turn", "Extended_strand", "Random_coil"),
  variable.name = "Structure",
  value.name = "Percentage"
)

# 修改图例显示名称（美观、符合论文格式）
levels(data_long$Structure) <- c(
  "α-Helix",
  "β-Turn",
  "Extended strand",
  "Random coil"
)

# ===================== 绘图 =====================
p <- ggplot(data_long, aes(x = GeneID, y = Percentage, color = Structure, group = Structure)) +
  geom_line(linewidth = 0.9) +
  geom_point(size = 1) +
  scale_y_continuous(
    limits = c(0, 70),
    breaks = seq(0, 70, 10),
    labels = paste0(seq(0, 70, 10), "%")
  ) +
  scale_color_manual(
    values = c("#787ef5", "#e8f578", "#a2f595", "#89f0e2")
  ) +
  labs(
    x = "",
    y = "Secondary structure percentage (%)",
    color = ""
  ) +
  theme_classic() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1, size = 6),
    axis.text.y = element_text(size = 11),
    axis.title.y = element_text(size = 12),
    legend.position = "top",
    legend.text = element_text(size = 11),
    panel.grid.major.y = element_line(color = "gray90", linewidth = 0.5)
  )

print(p)

# 保存高清图片（论文可用）
ggsave("protein_structure.png", p, width = 10, height = 6, dpi = 300)
