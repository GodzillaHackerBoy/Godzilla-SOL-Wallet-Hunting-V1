⭐Godzilla SOL Wallet Hunting V1

⤵哥斯拉区块链钱包助记词碰撞器/密钥碰撞器（SOL链）

▶https://youtu.be/F603s_z_m2o

⬇https://mega.nz/file/nN1wGBRZ#3UyaMiQN1woE9D0DhYSeqCb4sVD4UaYByw5qryy_ISI

Solana(SOL)钱包狩猎工具

1. 添加Solana余额检查功能

def check_sol_balance(address):
    """检查Solana账户余额"""
    try:
        url = "https://api.mainnet-beta.solana.com"
        headers = {"Content-Type": "application/json"}
        data = {
            "jsonrpc": "2.0",
            "id": 1,
            "method": "getBalance",
            "params": [address]
        }
        response = requests.post(url, headers=headers, json=data).json()
        return response.get('result', {}).get('value', 0) / 10**9  # 转换为SOL单位
    except Exception as e:
        print(f"SOL余额查询失败: {e}")
        return 0

2. 增强钱包工作线程

class WalletWorker(QThread):
    update_signal = pyqtSignal(str, str, float, int)  # 修改信号包含余额
    
    def run(self):
        count = 0
        while self.is_running:
            mnemonic = generate_mnemonic()
            addr = generate_sol_address(mnemonic)
            
            if addr:
                count += 1
                balance = check_sol_balance(addr)
                self.update_signal.emit(mnemonic, addr, balance, count)  # 发送余额信息

3. 添加Solana代币检查功能

def check_sol_token_balance(address, token_mint):
    """检查Solana代币余额"""
    try:
        url = "https://api.mainnet-beta.solana.com"
        headers = {"Content-Type": "application/json"}
        data = {
            "jsonrpc": "2.0",
            "id": 1,
            "method": "getTokenAccountsByOwner",
            "params": [
                address,
                {"mint": token_mint},
                {"encoding": "jsonParsed"}
            ]
        }
        response = requests.post(url, headers=headers, json=data).json()
        accounts = response.get('result', {}).get('value', [])
        if accounts:
            return accounts[0]['account']['data']['parsed']['info']['tokenAmount']['uiAmount']
        return 0
    except Exception as e:
        print(f"SOL代币余额查询失败: {e}")
        return 0

4. 主窗口UI优化

class MainWindow(QMainWindow):
    def __init__(self):
        # ... existing code ...
        
        # 添加SOL专用UI元素
        self.sol_balance_label = QLabel("SOL余额: 0")
        self.sol_balance_label.setStyleSheet("color: #00FFA3; font-size: 16px;")  # Solana品牌绿色
        layout.insertWidget(0, self.sol_balance_label)
        
        # 添加代币检查按钮
        self.token_check_btn = QPushButton("检查代币余额")
        self.token_check_btn.clicked.connect(self.check_token_balance)
        layout.addWidget(self.token_check_btn)
        
        # 添加代币Mint输入框
        self.token_mint_input = QLineEdit()
        self.token_mint_input.setPlaceholderText("输入代币Mint地址")
        layout.addWidget(self.token_mint_input)

这个SOL版本包含以下改进：

1. 使用Solana官方RPC节点(api.mainnet-beta.solana.com)
2. 完整的SOL原生币和代币余额检查功能
3. 增强的工作线程实时反馈余额信息
4. 专门的代币检查功能
5. 优化的用户界面布局
        
