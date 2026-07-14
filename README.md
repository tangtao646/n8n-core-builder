#验证资源包是否有效

NODE_BIN="$HOME/Library/Application Support/com.mrtang.n8n/runtime/bin/node"
N8N_BIN="$HOME/Library/Application Support/com.mrtang.n8n/n8n-core/node_modules/n8n/bin/n8n"


echo "==== 1. 检查文件就位情况 ===="
if [ -f "$NODE_BIN" ]; then echo "✅ Node 运行时存在: $($NODE_BIN --version)"; else echo "❌ Node 运行时不存在！"; fi
if [ -f "$N8N_BIN" ]; then echo "✅ n8n 执行入口存在"; else echo "❌ n8n 执行入口不存在！"; fi


echo "==== 2. 尝试静态加载依赖（测试模块完整性） ===="
"$NODE_BIN" -e "try { require('$HOME/Library/Application Support/com.mrtang.n8n/n8n-core/node_modules/n8n'); console.log('✅ 依赖包完整，可以被 Node.js 成功 Resolve！'); } catch(e) { console.error('❌ 依赖链加载失败，报错详情:', e.message); }"


echo "==== 3. 强制物理启动 n8n 服务 ===="
cd "$HOME/Library/Application Support/com.mrtang.n8n/n8n-core" && "$NODE_BIN" "$N8N_BIN" start
