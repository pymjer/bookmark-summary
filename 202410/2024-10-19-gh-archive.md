# GH Archive
- URL: https://www.gharchive.org/
- Added At: 2024-10-19 00:36:09
- [Link To Text](2024-10-19-gh-archive_raw.md)

## TL;DR
GH Archive项目记录并归档GitHub公共时间线数据，支持开源开发。它提供15多种事件类型，数据每小时更新一次，可通过HTTP客户端获取。数据以JSON格式存储，用户可下载并自定义处理，如使用Ruby脚本进行下载和遍历。

## Summary
1. **GH Archive项目介绍**：
   - GH Archive旨在记录和归档GitHub公共时间线，使其易于访问和分析。
   - 项目支持开源开发者，记录全球数百万项目的开发活动。

2. **GitHub事件类型**：
   - GitHub提供15多种事件类型，包括提交、分支、开票、评论和添加项目成员等。
   - 这些事件被聚合到每小时的存档中，可通过HTTP客户端访问。

3. **数据访问命令**：
   - 查询2015年1月1日下午3点UTC的活动：
     ```
     wget https://data.gharchive.org/2015-01-01-15.json.gz
     ```
   - 查询2015年1月1日全天的活动：
     ```
     wget https://data.gharchive.org/2015-01-01-{0..23}.json.gz
     ```
   - 查询2015年1月整月的活动：
     ```
     wget https://data.gharchive.org/2015-01-{01..31}-{0..23}.json.gz
     ```

4. **数据格式**：
   - 每个存档包含由GitHub API报告的JSON编码事件。
   - 用户可以下载原始数据并进行自定义处理，如编写聚合脚本、导入数据库等。

5. **示例脚本**：
   - 提供了一个Ruby脚本示例，用于下载和遍历单个存档。
