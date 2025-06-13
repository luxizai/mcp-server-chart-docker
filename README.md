# MCP Server Chart  ![](https://badge.mcpx.dev?type=server 'MCP Server')  [![build](https://github.com/antvis/mcp-server-chart/actions/workflows/build.yml/badge.svg)](https://github.com/antvis/mcp-server-chart/actions/workflows/build.yml) [![npm Version](https://img.shields.io/npm/v/@antv/mcp-server-chart.svg)](https://www.npmjs.com/package/@antv/mcp-server-chart) [![smithery badge](https://smithery.ai/badge/@antvis/mcp-server-chart)](https://smithery.ai/server/@antvis/mcp-server-chart) [![npm License](https://img.shields.io/npm/l/@antv/mcp-server-chart.svg)](https://www.npmjs.com/package/@antv/mcp-server-chart)

A Model Context Protocol server for generating charts using [AntV](https://github.com/antvis/).

<a href="https://www.star-history.com/#antvis/mcp-server-chart&Date">
  <img width="512" src="https://api.star-history.com/svg?repos=antvis/mcp-server-chart&type=Date" />
</a>

This is a TypeScript-based MCP server that provides chart generation capabilities. It allows you to create various types of charts through MCP tools. You can also use it in [Dify](https://marketplace.dify.ai/plugins/antv/visualization).


## ✨ Features

Now 20+ charts supported.

<img width="640" alt="mcp-server-chart preview" src="https://mdn.alipayobjects.com/huamei_qa8qxu/afts/img/A*ZlzKQKoJzsYAAAAAAAAAAAAAemJ7AQ/fmt.webp" />

- `generate_area_chart` - Generate a `area` chart, and return an image URL.
- `generate_bar_chart` - Generate a `bar` chart, and return an image URL.
- `generate_boxplot_chart` - Generate a `boxplot` chart, and return an image URL. 
- `generate_column_chart` - Generate a `column` chart, and return an image URL.
- `generate_dual_axes_chart` - Generate a `dual-axes` chart, and return an image URL.
- `generate_fishbone_diagram` - Generate a `fishbone-diagram` chart, and return an image URL.
- `generate_flow_diagram` - Generate a `flow-diagram` chart, and return an image URL.
- `generate_funnel_chart` - Generate a `funnel` chart, and return an image URL.
- `generate_histogram_chart` - Generate a `histogram` chart, and return an image URL.
- `generate_line_chart` - Generate a `line` chart, and return an image URL.
- `generate_liquid_chart` - Generate a `liquid` chart, and return an image URL.
- `generate_mind_map` - Generate a `mind-map` chart, and return an image URL.
- `generate_network_graph` - Generate a `network-graph` chart, and return an image URL.
- `generate_organization_chart` - Generate a `organization-chart` chart, and return an image URL.
- `generate_pie_chart` - Generate a `pie` chart, and return an image URL.
- `generate_radar_chart` - Generate a `radar` chart, and return an image URL.
- `generate_sankey_chart` - Generate a `sankey` chart, and return an image URL.
- `generate_scatter_chart` - Generate a `scatter` chart, and return an image URL.
- `generate_treemap_chart` - Generate a `treemap` chart, and return an image URL.
- `generate_venn_chart` - Generate a `venn` chart, and return an image URL.
- `generate_violin_chart` - Generate a `violin` chart, and return an image URL.
- `generate_word_cloud_chart` - Generate a `word-cloud` chart, and return an image URL.


## 🤖 Usage

To use with `Desktop APP`, such as Claude, VSCode, [Cline](https://cline.bot/mcp-marketplace), Cherry Studio, Cursor, and so on, add the MCP server config below. On Mac system:

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "command": "npx",
      "args": [
        "-y",
        "@antv/mcp-server-chart"
      ]
    }
  }
}
```

On Window system:

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@antv/mcp-server-chart"
      ]
    }
  }
}
```

Also, you can use it on [aliyun](https://bailian.console.aliyun.com/?tab=mcp#/mcp-market/detail/antv-visualization-chart), [modelscope](https://www.modelscope.cn/mcp/servers/@antvis/mcp-server-chart), [glama.ai](https://glama.ai/mcp/servers/@antvis/mcp-server-chart), [smithery.ai](https://smithery.ai/server/@antvis/mcp-server-chart) or others with HTTP, SSE Protocol.


## 🚰 Run with SSE or Streamable transport

Install the package globally.

```bash
npm install -g @antv/mcp-server-chart
```

Run the server with your preferred transport option:

```bash
# For SSE transport (default endpoint: /sse)
mcp-server-chart --transport sse

# For Streamable transport with custom endpoint
mcp-server-chart --transport streamable
```

Then you can access the server at:
- SSE transport: `http://localhost:1122/sse`
- Streamable transport: `http://localhost:1122/mcp`


## 🎮 CLI Options

You can also use the following CLI options when running the MCP server. Command options by run cli with `-h`.

```plain
MCP Server Chart CLI

Options:
  --transport, -t  Specify the transport protocol: "stdio", "sse", or "streamable" (default: "stdio")
  --port, -p       Specify the port for SSE or streamable transport (default: 1122)
  --endpoint, -e   Specify the endpoint for the transport:
                   - For SSE: default is "/sse"
                   - For streamable: default is "/mcp"
  --help, -h       Show this help message
```


## 📠 Private Deployment

`MCP Server Chart` provides a free chart generation service by default. For users with a need for private deployment, they can try using `VIS_REQUEST_SERVER` to customize their own chart generation service.

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "command": "npx",
      "args": [
        "-y",
        "@antv/mcp-server-chart"
      ],
      "env": {
        "VIS_REQUEST_SERVER": "<YOUR_VIS_REQUEST_SERVER>"
      }
    }
  }
}
```

You can use AntV's project [GPT-Vis-SSR](https://github.com/antvis/GPT-Vis/tree/main/bindings/gpt-vis-ssr) to deploy an HTTP service in a private environment, and then pass the URL address through env `VIS_REQUEST_SERVER`.

- **Method**: `POST`
- **Parameter**: Which will be passed to `GPT-Vis-SSR` for renderring. Such as, `{ "type": "line", "data": [{ "time": "2025-05", "value": "512" }, { "time": "2025-06", "value": "1024" }] }`.
- **Return**: The return object of HTTP service.
  - **success**: `boolean` Whether generate chart image successfully.
  - **resultObj**: `string` The chart image url.
  - **errorMessage**: `string` When `success = false`, return the error message.


## 🔨 Development

Install dependencies:

```bash
npm install
```

Build the server:

```bash
npm run build
```

Start the MCP server:

```bash
npm run start
```


## 📄 License

MIT@[AntV](https://github.com/antvis).
