# 装修追踪更新 Skill

## 项目信息
- **项目名称**: 美加广场 E203 装修需求追踪
- **设计师**: 刘工
- **GitHub 仓库**: https://github.com/astarzf/angiespace.git
- **本地文件路径**: /workspace/renovation-tracker/
- **核心文件**: /workspace/renovation-tracker/index.html
- **GitHub Pages URL**: https://astarzf.github.io/angiespace/
- **数据版本**: 2026-08-01

## 数据结构

### 需求对象 (Requirement)
```json
{
  "id": 1,                    // 唯一ID，新增时递增
  "area": "客厅卫生间",         // 区域
  "category": "隔断设计",      // 需求类别
  "desc": "需求描述",          // 详细描述
  "date": "2026-07",          // 提出日期
  "priority": "高/中/低",      // 优先级
  "status": "需修改/待确认/待评估/待出图/待设计/待提供/已确认/已完成/已搁置",
  "feedback": "",             // 设计师反馈
  "note": ""                  // 备注
}
```

### 状态定义
| 状态 | 含义 | 颜色 |
|------|------|------|
| 需修改 | 设计师需修改的方案 | 红色 |
| 待确认 | 需设计师确认方案 | 黄色 |
| 待评估 | 需设计师评估可行性 | 黄色 |
| 待出图 | 需设计师出效果图 | 蓝色 |
| 待设计 | 需设计师设计方案 | 蓝色 |
| 待提供 | 需设计师提供资料/数据 | 蓝色 |
| 已确认 | 已确定方案 | 绿色 |
| 已完成 | 已完成 | 绿色 |
| 已搁置 | 暂时搁置 | 灰色 |

### 沟通日志对象 (Log)
```json
{
  "date": "2026-08-01",
  "method": "微信群",
  "reqIds": "1,2,3",
  "summary": "沟通内容摘要",
  "reply": "设计师回复/下一步"
}
```

## 当前需求清单 (ID映射)
| ID | 区域 | 简述 |
|----|------|------|
| 1 | 客厅卫生间 | 马桶与淋浴间不做隔断 |
| 2 | 客厅卫生间 | 双开门入口 |
| 3 | 客厅卫生间 | 马桶前封墙/隔断 |
| 4 | 客厅卫生间 | 淋浴间门开淋浴侧 |
| 5 | 客厅卫生间 | 不做玻璃门 |
| 6 | 客厅卫生间 | 百叶门替代玻璃门 |
| 7 | 客厅卫生间 | 淋浴区下沉设计 |
| 8 | 客厅卫生间 | 自然风格 |
| 9 | 晾晒区 | 安装晴空灯 |
| 10 | 主卧卫生间 | 复古风格 |
| 11 | 主卧卫生间 | 马赛克软质地砖 |
| 12 | 主卧卫生间 | 衣帽间区域设计 |
| 13 | 主卧卫生间 | 男生洗手池复古软装 |
| 14 | 主卧卫生间 | 出效果图 |
| 15 | 客餐厅 | 木纹砖人字拼 |
| 16 | 全屋 | 门窗尺寸清单 |
| 17 | 客餐厅 | 窗户格子对齐修改 |
| 18 | 客餐厅 | 阳台门关闭效果图 |
| 19 | 客餐厅 | 窗外风景改二楼树景 |
| 20 | 走廊/书房 | 储物柜对开门修改 |
| 21 | 走廊 | 走廊效果图 |
| 22 | 阳台 | 阳台地砖设计 |
| 23 | 阳台 | 铁树换柠檬树 |
| 24 | 阳台 | 阳台开门效果图 |

## 更新流程

### 当用户发送新的装修沟通进展时：

1. **解析用户输入**
   - 如果是图片：读取图片提取文字内容
   - 如果是文字：直接解析
   - 识别：新增需求 / 状态变更 / 设计师反馈

2. **更新数据**
   - 新增需求：分配新ID（当前最大ID+1），填写所有字段
   - 状态变更：更新对应ID的status字段
   - 设计师反馈：更新对应ID的feedback字段
   - 添加沟通日志到logs数组

3. **更新HTML文件**
   - 修改 `/workspace/renovation-tracker/index.html` 中的 `BASE_DATA` 对象
   - 更新 `version` 为当前日期（YYYY-MM-DD）
   - 更新 `lastUpdate` 为当前日期
   - 更新 `requirements` 数组
   - 更新 `logs` 数组

4. **推送到GitHub**
   ```bash
   cd /workspace/renovation-tracker
   git add -A
   git commit -m "更新装修追踪 - YYYY-MM-DD"
   git push origin main
   ```

5. **验证**
   - 确认push成功
   - 提示用户在iPhone上刷新查看

## Git配置
```bash
git config --global user.email "renovation-tracker@local"
git config --global user.name "Angie"
git remote add origin https://ghp_xxx@github.com/astarzf/angiespace.git
```

## 注意事项
- **版本合并机制**: HTML中内置了智能合并逻辑。更新BASE_DATA的version字段后，用户的本地编辑（localStorage）会自动合并到新版本中，不会丢失
- **ID唯一性**: 新增需求时，ID必须是当前最大ID+1，不可重复
- **数据格式**: BASE_DATA是JSON对象，嵌在HTML的script标签中，更新时注意保持JSON格式正确
- **字符转义**: 需求描述中如有引号，需用转义字符
