# MCP Server Chart — Docker

基于 [AntV mcp-server-chart](https://github.com/antvis/mcp-server-chart) 封装的 **Docker 镜像**，以最简方式快速部署一个可供 AI 客户端（Claude、Cursor、Cherry Studio、Cline 等）调用的 MCP 图表生成服务，支持 26+ 种图表类型。

可用的图表能力与原项目完全一致，包括折线图、柱状图、饼图、雷达图、桑基图、词云、思维导图、流程图、组织结构图等，详见下方 [支持的图表](#-支持的图表)。

## 📦 镜像

| 项 | 说明 |
| --- | --- |
| 镜像名 | `ghcr.io/luxizai/mcp-server-chart-docker:latest` |
| 默认命令 | `mcp-server-chart --transport streamable --port 1122 --host 0.0.0.0` |
| 默认端口 | `1122`（Streamable 传输，端点 `/mcp`） |
| 基础镜像 | `node:lts` |

## 🚀 快速开始

### Docker Run

```bash
docker run -d \
  --name mcp-server-chart \
  -p 1122:1122 \
  --restart unless-stopped \
  ghcr.io/luxizai/mcp-server-chart-docker:latest
```

### Docker Compose

```bash
docker compose up -d
```

启动后服务地址为 `http://localhost:1122/mcp`。

## 🤖 接入 MCP 客户端

在支持 HTTP / Streamable 传输的 MCP 客户端（如 Claude Desktop、Cursor、Cherry Studio 等）中添加：

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "url": "http://localhost:1122/mcp"
    }
  }
}
```

不同客户端的配置入口不同，通常为 `mcpServers` 配置文件中新增上述条目，然后重启客户端即可。详细连接方式可参考原项目 [AntV mcp-server-chart](https://github.com/antvis/mcp-server-chart)。

## ⚙️ 环境变量

可以通过环境变量传入原项目支持的配置：

| 变量 | 说明 | 默认值 |
| --- | --- | --- |
| `VIS_REQUEST_SERVER` | 自定义图表生成服务地址（私有化部署） | `https://antv-studio.alipay.com/api/gpt-vis` |
| `SERVICE_ID` | 图表生成记录的服务标识 | - |
| `DISABLED_TOOLS` | 逗号分隔，禁用指定图表工具 | - |

以 `docker run` 为例：

```bash
docker run -d \
  -p 1122:1122 \
  -e DISABLED_TOOLS="generate_fishbone_diagram,generate_mind_map" \
  -e VIS_REQUEST_SERVER="https://your-server.com/api/chart" \
  ghcr.io/luxizai/mcp-server-chart-docker:latest
```

对应 `docker-compose.yml`：

```yaml
services:
  mcp-server-chart:
    image: ghcr.io/luxizai/mcp-server-chart-docker:latest
    ports:
      - "1122:1122"
    environment:
      - DISABLED_TOOLS=generate_fishbone_diagram,generate_mind_map
```

## 🏗️ 本地构建

```bash
docker build -t mcp-server-chart-docker .
docker run -d -p 1122:1122 mcp-server-chart-docker
```

推送 `main` 分支时，GitHub Actions（`.github/workflows/docker-image.yml`）会自动构建并推送镜像到 GHCR。

## 📊 支持的图表

镜像内置原项目全部图表生成工具（26+ 种）：

`generate_area_chart`、`generate_bar_chart`、`generate_boxplot_chart`、`generate_column_chart`、`generate_district_map`、`generate_dual_axes_chart`、`generate_fishbone_diagram`、`generate_flow_diagram`、`generate_funnel_chart`、`generate_histogram_chart`、`generate_line_chart`、`generate_liquid_chart`、`generate_mind_map`、`generate_network_graph`、`generate_organization_chart`、`generate_path_map`、`generate_pie_chart`、`generate_pin_map`、`generate_radar_chart`、`generate_sankey_chart`、`generate_scatter_chart`、`generate_treemap_chart`、`generate_venn_chart`、`generate_violin_chart`、`generate_word_cloud_chart`、`generate_spreadsheet`。

> [!NOTE]
> 地理类图表（`generate_district_map`、`generate_path_map`、`generate_pin_map`）依赖[高德地图服务](https://lbs.amap.com/)，目前仅支持中国范围内地图生成。

更多工具用法与数据格式参见原项目 [AntV mcp-server-chart](https://github.com/antvis/mcp-server-chart)。

## 🔍 健康检查

容器内置健康检查（每 60s 探测一次 `http://localhost:1122/health`），亦可在宿主机手动验证：

```bash
curl http://localhost:1122/health
```

## 📄 致谢与许可

本项目仅是对 [AntV mcp-server-chart](https://github.com/antvis/mcp-server-chart)（MIT © AntV）的 Docker 封装，核心能力与版权归原项目所有。