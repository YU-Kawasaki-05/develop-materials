# 暗号化とハッシュ化：現代暗号学の理論と実装

## 🎯 この章で学ぶこと
- 暗号学の数学的基礎と理論的背景の理解
- 対称暗号化と非対称暗号化の詳細な仕組みと実装
- ハッシュ関数の原理と暗号学的用途の習得
- デジタル署名とPKI（公開鍵基盤）の包括的理解
- 現代暗号技術（AES、RSA、楕円曲線暗号）の実装と最適化
- 量子耐性暗号（ポスト量子暗号）への移行戦略
- 暗号プロトコル（TLS、SSH、IPSec）の内部構造と実装
- サイドチャネル攻撃対策と安全な暗号実装技術
- 暗号アジリティフレームワークの設計と運用
- エンタープライズ暗号管理システムの構築

## 🤔 なぜ重要なのか

### 現代社会における暗号化の不可欠性

**デジタル社会の基盤技術**

暗号化技術は現代のデジタル社会の根幹を支える基盤技術です。オンライン決済、プライベート通信、クラウドストレージ、IoTデバイス、医療記録管理など、私たちの生活のあらゆる場面で暗号化が使用されています。

```
暗号化技術の適用範囲（2024年現在）
┌─────────────────────────────────────────────────────┐
│ 金融セクター                                        │
│ ├── デジタル決済: 1日あたり5兆円以上のトランザクション│
│ ├── 銀行間通信: SWIFT等の国際決済システム          │
│ └── 暗号通貨: Bitcoin、Ethereum等の分散台帳技術    │
│                                                     │
│ 通信・インターネット                                │
│ ├── HTTPS通信: 全Webトラフィックの95%以上が暗号化   │
│ ├── メッセージング: WhatsApp、Signal等のE2E暗号化   │
│ └── 5G通信: 超高速通信の暗号化プロトコル            │
│                                                     │
│ 企業・組織                                          │
│ ├── クラウドストレージ: 数百ペタバイトの暗号化データ │
│ ├── VPN接続: リモートワークの安全な接続基盤        │
│ └── 機密情報管理: 国家機密、企業秘密の保護         │
└─────────────────────────────────────────────────────┘
```

**量子コンピュータ時代の到来**

2024年現在、量子コンピュータの実用化が現実的な脅威となっています。Google、IBM、IonQなどの企業が量子優位性を達成し、従来の暗号化技術に対する深刻な脅威となっています。

**具体的な影響予測**
- **2030年代前半**: 中規模量子コンピュータの実用化
- **RSA-2048の破綻**: 現在の公開鍵暗号の安全性が失われる
- **楕円曲線暗号の脆弱化**: 現在広く使用されている暗号が無効化
- **対称暗号の安全性低下**: AES-128が実質的にAES-64相当に

### 実際の暗号化失敗事例とその影響

**Equifax データ侵害事件（2017年）**
```
原因: 暗号化の不適切な実装
├── 機密データの平文保存
├── 古い暗号化アルゴリズム（DES）の使用
├── 鍵管理システムの脆弱性
└── 暗号化されていない通信チャネル

影響:
├── 1億4,300万人の個人情報流出
├── 企業価値の40%減少（約4兆円）
├── 法的制裁金700億円
└── 信頼回復に7年以上を要する
```

**SolarWinds サプライチェーン攻撃（2020年）**
```
攻撃手法: デジタル署名の偽造
├── 正規のソフトウェア更新に悪意あるコードを埋め込み
├── 偽造されたデジタル署名による信頼性の悪用
├── 暗号化された通信チャネルでの潜伏
└── 18,000以上の組織に影響

教訓:
├── デジタル署名の検証プロセスの重要性
├── 暗号化鍵の適切な管理
├── ハードウェアセキュリティモジュール（HSM）の必要性
└── 暗号アジリティの重要性
```

## 📚 基礎概念の理解

### 暗号学の基本原理

**暗号学の基本概念**

暗号学は情報を保護するための数学的技術の集合です。その基本目標は「機密性」「完全性」「認証」「否認防止」の4つです。

```python
"""
暗号学の基本概念実装
"""

from abc import ABC, abstractmethod
from typing import Union, Tuple, Optional
import hashlib
import secrets
import os
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend

class CryptographicPrimitive(ABC):
    """暗号学的プリミティブの基底クラス"""
    
    @abstractmethod
    def encrypt(self, plaintext: bytes) -> bytes:
        """暗号化の実装"""
        pass
    
    @abstractmethod
    def decrypt(self, ciphertext: bytes) -> bytes:
        """復号化の実装"""
        pass
    
    @abstractmethod
    def generate_key(self) -> bytes:
        """鍵生成の実装"""
        pass

class SymmetricCryptography(CryptographicPrimitive):
    """対称暗号化システム"""
    
    def __init__(self, algorithm: str = "AES-256-GCM"):
        self.algorithm = algorithm
        self.key_size = self._get_key_size(algorithm)
        self.backend = default_backend()
    
    def _get_key_size(self, algorithm: str) -> int:
        """アルゴリズムに応じた鍵長の取得"""
        key_sizes = {
            "AES-128-GCM": 16,
            "AES-192-GCM": 24,
            "AES-256-GCM": 32,
            "AES-128-CBC": 16,
            "AES-256-CBC": 32,
            "ChaCha20-Poly1305": 32
        }
        return key_sizes.get(algorithm, 32)
    
    def generate_key(self) -> bytes:
        """暗号学的に安全な鍵の生成"""
        return secrets.token_bytes(self.key_size)
    
    def encrypt_with_aes_gcm(self, plaintext: bytes, key: bytes, 
                           associated_data: bytes = None) -> Tuple[bytes, bytes, bytes]:
        """AES-GCMによる認証付き暗号化"""
        
        # 初期化ベクトル（IV）の生成
        iv = os.urandom(12)  # GCMモードでは96ビット（12バイト）のIVを使用
        
        # 暗号器の初期化
        cipher = Cipher(
            algorithms.AES(key),
            modes.GCM(iv),
            backend=self.backend
        )
        encryptor = cipher.encryptor()
        
        # 関連データの設定（認証に使用するが暗号化されない）
        if associated_data:
            encryptor.authenticate_additional_data(associated_data)
        
        # 暗号化の実行
        ciphertext = encryptor.update(plaintext) + encryptor.finalize()
        
        # 認証タグの取得
        tag = encryptor.tag
        
        return ciphertext, iv, tag
    
    def decrypt_with_aes_gcm(self, ciphertext: bytes, key: bytes, 
                           iv: bytes, tag: bytes, 
                           associated_data: bytes = None) -> bytes:
        """AES-GCMによる認証付き復号化"""
        
        # 復号器の初期化
        cipher = Cipher(
            algorithms.AES(key),
            modes.GCM(iv, tag),
            backend=self.backend
        )
        decryptor = cipher.decryptor()
        
        # 関連データの設定
        if associated_data:
            decryptor.authenticate_additional_data(associated_data)
        
        # 復号化の実行（認証も同時に行われる）
        try:
            plaintext = decryptor.update(ciphertext) + decryptor.finalize()
            return plaintext
        except Exception as e:
            raise ValueError(f"復号化に失敗しました（認証エラーの可能性）: {e}")
    
    def encrypt_with_chacha20_poly1305(self, plaintext: bytes, key: bytes) -> Tuple[bytes, bytes]:
        """ChaCha20-Poly1305による認証付き暗号化"""
        
        from cryptography.hazmat.primitives.ciphers.aead import ChaCha20Poly1305
        
        # ChaCha20Poly1305インスタンスの作成
        chacha = ChaCha20Poly1305(key)
        
        # ナンスの生成（ChaCha20では96ビット）
        nonce = os.urandom(12)
        
        # 暗号化の実行
        ciphertext = chacha.encrypt(nonce, plaintext, None)
        
        return ciphertext, nonce
    
    def decrypt_with_chacha20_poly1305(self, ciphertext: bytes, key: bytes, nonce: bytes) -> bytes:
        """ChaCha20-Poly1305による認証付き復号化"""
        
        from cryptography.hazmat.primitives.ciphers.aead import ChaCha20Poly1305
        
        # ChaCha20Poly1305インスタンスの作成
        chacha = ChaCha20Poly1305(key)
        
        # 復号化の実行
        try:
            plaintext = chacha.decrypt(nonce, ciphertext, None)
            return plaintext
        except Exception as e:
            raise ValueError(f"復号化に失敗しました: {e}")
    
    def key_derivation_pbkdf2(self, password: str, salt: bytes, iterations: int = 100000) -> bytes:
        """PBKDF2による鍵導出"""
        
        from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
        
        # パスワードをバイト列に変換
        password_bytes = password.encode('utf-8')
        
        # PBKDF2インスタンスの作成
        kdf = PBKDF2HMAC(
            algorithm=hashes.SHA256(),
            length=self.key_size,
            salt=salt,
            iterations=iterations,
            backend=self.backend
        )
        
        # 鍵の導出
        key = kdf.derive(password_bytes)
        return key
    
    def key_derivation_scrypt(self, password: str, salt: bytes, 
                            n: int = 16384, r: int = 8, p: int = 1) -> bytes:
        """scryptによる鍵導出（GPU攻撃耐性）"""
        
        from cryptography.hazmat.primitives.kdf.scrypt import Scrypt
        
        password_bytes = password.encode('utf-8')
        
        # scryptインスタンスの作成
        kdf = Scrypt(
            algorithm=hashes.SHA256(),
            length=self.key_size,
            salt=salt,
            n=n,
            r=r,
            p=p,
            backend=self.backend
        )
        
        key = kdf.derive(password_bytes)
        return key
    
    def encrypt(self, plaintext: bytes) -> bytes:
        """基本的な暗号化インターフェース"""
        key = self.generate_key()
        ciphertext, iv, tag = self.encrypt_with_aes_gcm(plaintext, key)
        
        # 結果を連結（実際のシステムでは適切な形式で格納）
        return key + iv + tag + ciphertext
    
    def decrypt(self, encrypted_data: bytes) -> bytes:
        """基本的な復号化インターフェース"""
        key = encrypted_data[:self.key_size]
        iv = encrypted_data[self.key_size:self.key_size+12]
        tag = encrypted_data[self.key_size+12:self.key_size+28]
        ciphertext = encrypted_data[self.key_size+28:]
        
        return self.decrypt_with_aes_gcm(ciphertext, key, iv, tag)

class AsymmetricCryptography:
    """非対称暗号化システム"""
    
    def __init__(self, key_size: int = 2048):
        self.key_size = key_size
        self.backend = default_backend()
    
    def generate_rsa_key_pair(self) -> Tuple[bytes, bytes]:
        """RSA鍵ペアの生成"""
        
        # 秘密鍵の生成
        private_key = rsa.generate_private_key(
            public_exponent=65537,  # 標準的な公開指数
            key_size=self.key_size,
            backend=self.backend
        )
        
        # 公開鍵の取得
        public_key = private_key.public_key()
        
        # PEM形式でのシリアライズ
        private_pem = private_key.private_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PrivateFormat.PKCS8,
            encryption_algorithm=serialization.NoEncryption()
        )
        
        public_pem = public_key.public_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PublicFormat.SubjectPublicKeyInfo
        )
        
        return private_pem, public_pem
    
    def encrypt_with_rsa(self, plaintext: bytes, public_key_pem: bytes) -> bytes:
        """RSAによる公開鍵暗号化"""
        
        # 公開鍵のロード
        public_key = serialization.load_pem_public_key(public_key_pem, backend=self.backend)
        
        # OAEP パディングを使用した暗号化
        ciphertext = public_key.encrypt(
            plaintext,
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        
        return ciphertext
    
    def decrypt_with_rsa(self, ciphertext: bytes, private_key_pem: bytes) -> bytes:
        """RSAによる秘密鍵復号化"""
        
        # 秘密鍵のロード
        private_key = serialization.load_pem_private_key(
            private_key_pem, 
            password=None, 
            backend=self.backend
        )
        
        # OAEP パディングを使用した復号化
        plaintext = private_key.decrypt(
            ciphertext,
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        
        return plaintext
    
    def sign_with_rsa(self, message: bytes, private_key_pem: bytes) -> bytes:
        """RSAによるデジタル署名"""
        
        # 秘密鍵のロード
        private_key = serialization.load_pem_private_key(
            private_key_pem,
            password=None,
            backend=self.backend
        )
        
        # PSS パディングを使用した署名
        signature = private_key.sign(
            message,
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        
        return signature
    
    def verify_rsa_signature(self, message: bytes, signature: bytes, public_key_pem: bytes) -> bool:
        """RSA署名の検証"""
        
        # 公開鍵のロード
        public_key = serialization.load_pem_public_key(public_key_pem, backend=self.backend)
        
        try:
            # PSS パディングを使用した署名検証
            public_key.verify(
                signature,
                message,
                padding.PSS(
                    mgf=padding.MGF1(hashes.SHA256()),
                    salt_length=padding.PSS.MAX_LENGTH
                ),
                hashes.SHA256()
            )
            return True
        except Exception:
            return False

class CryptographicHashFunction:
    """暗号学的ハッシュ関数"""
    
    def __init__(self):
        self.backend = default_backend()
    
    def sha256_hash(self, data: bytes) -> bytes:
        """SHA-256ハッシュ値の計算"""
        digest = hashes.Hash(hashes.SHA256(), backend=self.backend)
        digest.update(data)
        return digest.finalize()
    
    def sha3_256_hash(self, data: bytes) -> bytes:
        """SHA-3-256ハッシュ値の計算"""
        digest = hashes.Hash(hashes.SHA3_256(), backend=self.backend)
        digest.update(data)
        return digest.finalize()
    
    def blake2b_hash(self, data: bytes, key: bytes = None) -> bytes:
        """BLAKE2bハッシュ値の計算（鍵付きハッシュ対応）"""
        
        if key:
            digest = hashes.Hash(hashes.BLAKE2b(64, key=key), backend=self.backend)
        else:
            digest = hashes.Hash(hashes.BLAKE2b(64), backend=self.backend)
        
        digest.update(data)
        return digest.finalize()
    
    def hmac_sha256(self, message: bytes, key: bytes) -> bytes:
        """HMAC-SHA256の計算"""
        
        from cryptography.hazmat.primitives import hmac
        
        h = hmac.HMAC(key, hashes.SHA256(), backend=self.backend)
        h.update(message)
        return h.finalize()
    
    def verify_hmac(self, message: bytes, key: bytes, expected_hmac: bytes) -> bool:
        """HMACの検証"""
        
        from cryptography.hazmat.primitives import hmac
        
        h = hmac.HMAC(key, hashes.SHA256(), backend=self.backend)
        h.update(message)
        
        try:
            h.verify(expected_hmac)
            return True
        except Exception:
            return False

class SecureRandomGenerator:
    """暗号学的に安全な乱数生成"""
    
    def __init__(self):
        self.entropy_sources = self._initialize_entropy_sources()
    
    def _initialize_entropy_sources(self) -> dict:
        """エントロピー源の初期化"""
        return {
            "os_urandom": os.urandom,
            "secrets": secrets.token_bytes,
            "system_random": secrets.SystemRandom()
        }
    
    def generate_secure_random(self, length: int) -> bytes:
        """暗号学的に安全な乱数の生成"""
        return secrets.token_bytes(length)
    
    def generate_secure_random_int(self, min_val: int, max_val: int) -> int:
        """指定範囲の暗号学的に安全な乱数整数の生成"""
        return secrets.randbelow(max_val - min_val + 1) + min_val
    
    def generate_secure_password(self, length: int = 16, 
                               include_symbols: bool = True) -> str:
        """暗号学的に安全なパスワードの生成"""
        
        import string
        
        # 文字セットの定義
        charset = string.ascii_letters + string.digits
        if include_symbols:
            charset += "!@#$%^&*()_+-=[]{}|;:,.<>?"
        
        # パスワードの生成
        password = ''.join(secrets.choice(charset) for _ in range(length))
        
        return password
    
    def generate_salt(self, length: int = 16) -> bytes:
        """ソルトの生成"""
        return secrets.token_bytes(length)
    
    def generate_nonce(self, length: int = 12) -> bytes:
        """ナンスの生成"""
        return secrets.token_bytes(length)
```

### 対称暗号化の詳細実装

**Advanced Encryption Standard (AES) の実装**

AESは現在最も広く使用されている対称暗号化アルゴリズムです。128ビット、192ビット、256ビットの鍵長をサポートし、非常に高速で安全性が証明されています。

```python
"""
AES暗号化の詳細実装と最適化
"""

import time
import struct
from typing import List, Tuple, Optional, Dict
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend
import concurrent.futures
import mmap

class AdvancedAESImplementation:
    """高度なAES実装"""
    
    def __init__(self):
        self.backend = default_backend()
        self.performance_metrics = {}
    
    def aes_modes_comparison(self, plaintext: bytes, key: bytes) -> Dict[str, Dict]:
        """AES動作モードの比較評価"""
        
        modes_to_test = {
            "ECB": modes.ECB(),
            "CBC": modes.CBC(os.urandom(16)),
            "CTR": modes.CTR(os.urandom(16)),
            "GCM": modes.GCM(os.urandom(12)),
            "OFB": modes.OFB(os.urandom(16)),
            "CFB": modes.CFB(os.urandom(16))
        }
        
        results = {}
        
        for mode_name, mode in modes_to_test.items():
            start_time = time.time()
            
            try:
                # 暗号化
                cipher = Cipher(algorithms.AES(key), mode, backend=self.backend)
                encryptor = cipher.encryptor()
                ciphertext = encryptor.update(plaintext) + encryptor.finalize()
                
                # 復号化（新しいインスタンスで）
                if mode_name == "GCM":
                    # GCMモードの場合は認証タグが必要
                    tag = encryptor.tag
                    decrypt_mode = modes.GCM(mode.initialization_vector, tag)
                else:
                    decrypt_mode = type(mode)(mode.initialization_vector)
                
                cipher = Cipher(algorithms.AES(key), decrypt_mode, backend=self.backend)
                decryptor = cipher.decryptor()
                decrypted = decryptor.update(ciphertext) + decryptor.finalize()
                
                end_time = time.time()
                
                results[mode_name] = {
                    "success": True,
                    "ciphertext_length": len(ciphertext),
                    "processing_time": end_time - start_time,
                    "throughput_mbps": (len(plaintext) * 8) / ((end_time - start_time) * 1024 * 1024),
                    "authenticated": mode_name == "GCM",
                    "parallel_friendly": mode_name in ["ECB", "CTR", "GCM"]
                }
                
            except Exception as e:
                results[mode_name] = {
                    "success": False,
                    "error": str(e),
                    "processing_time": time.time() - start_time
                }
        
        return results
    
    def parallel_aes_encryption(self, data: bytes, key: bytes, 
                              chunk_size: int = 1024*1024) -> Tuple[bytes, List[bytes]]:
        """並列AES暗号化（大容量データ対応）"""
        
        # データをチャンクに分割
        chunks = [data[i:i+chunk_size] for i in range(0, len(data), chunk_size)]
        
        # 各チャンクのIVを生成
        ivs = [os.urandom(16) for _ in chunks]
        
        # 並列暗号化の実行
        with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
            futures = []
            for chunk, iv in zip(chunks, ivs):
                future = executor.submit(self._encrypt_chunk, chunk, key, iv)
                futures.append(future)
            
            encrypted_chunks = []
            for future in concurrent.futures.as_completed(futures):
                encrypted_chunks.append(future.result())
        
        # 結果の結合
        encrypted_data = b''.join(encrypted_chunks)
        return encrypted_data, ivs
    
    def _encrypt_chunk(self, chunk: bytes, key: bytes, iv: bytes) -> bytes:
        """チャンクの暗号化"""
        
        # CTRモードを使用（並列処理に適している）
        cipher = Cipher(algorithms.AES(key), modes.CTR(iv), backend=self.backend)
        encryptor = cipher.encryptor()
        return encryptor.update(chunk) + encryptor.finalize()
    
    def stream_aes_encryption(self, file_path: str, key: bytes, 
                            output_path: str, chunk_size: int = 8192):
        """ストリーミングAES暗号化（大容量ファイル対応）"""
        
        # 初期化ベクトルの生成
        iv = os.urandom(16)
        
        # 暗号器の初期化
        cipher = Cipher(algorithms.AES(key), modes.CBC(iv), backend=self.backend)
        encryptor = cipher.encryptor()
        
        with open(file_path, 'rb') as infile, open(output_path, 'wb') as outfile:
            # IVを最初に書き込み
            outfile.write(iv)
            
            # ストリーミング暗号化
            while True:
                chunk = infile.read(chunk_size)
                if not chunk:
                    break
                
                # 最後のチャンクの場合はパディングを追加
                if len(chunk) < chunk_size:
                    chunk = self._add_pkcs7_padding(chunk, 16)
                
                encrypted_chunk = encryptor.update(chunk)
                outfile.write(encrypted_chunk)
            
            # 最終化
            final_chunk = encryptor.finalize()
            if final_chunk:
                outfile.write(final_chunk)
    
    def _add_pkcs7_padding(self, data: bytes, block_size: int) -> bytes:
        """PKCS#7パディングの追加"""
        padding_length = block_size - (len(data) % block_size)
        padding = bytes([padding_length] * padding_length)
        return data + padding
    
    def _remove_pkcs7_padding(self, data: bytes) -> bytes:
        """PKCS#7パディングの除去"""
        padding_length = data[-1]
        return data[:-padding_length]
    
    def aes_key_expansion_analysis(self, key: bytes) -> Dict:
        """AES鍵拡張の分析"""
        
        from cryptography.hazmat.primitives.ciphers.algorithms import AES
        
        key_schedule_info = {
            "original_key": key.hex(),
            "key_length": len(key),
            "rounds": {
                128: 10,
                192: 12,
                256: 14
            }.get(len(key) * 8, 0),
            "total_round_keys": 0,
            "key_schedule_size": 0
        }
        
        if key_schedule_info["rounds"]:
            key_schedule_info["total_round_keys"] = key_schedule_info["rounds"] + 1
            key_schedule_info["key_schedule_size"] = key_schedule_info["total_round_keys"] * 16
        
        return key_schedule_info
    
    def side_channel_resistant_aes(self, plaintext: bytes, key: bytes) -> bytes:
        """サイドチャネル攻撃耐性AES実装"""
        
        # 定数時間実装の考慮事項
        implementation_notes = {
            "timing_attacks": "定数時間実装を使用",
            "power_analysis": "マスキング技術を適用",
            "cache_attacks": "キャッシュタイミング攻撃対策",
            "fault_injection": "エラー検出・訂正機能"
        }
        
        # 実際の実装では、専用のハードウェア（HSM）または
        # サイドチャネル攻撃耐性ライブラリを使用
        
        # 基本的な対策として、ランダムな遅延を追加
        import random
        time.sleep(random.uniform(0.001, 0.005))
        
        # 通常のAES暗号化
        iv = os.urandom(16)
        cipher = Cipher(algorithms.AES(key), modes.CBC(iv), backend=self.backend)
        encryptor = cipher.encryptor()
        
        # パディングを追加
        padded_plaintext = self._add_pkcs7_padding(plaintext, 16)
        
        # 暗号化
        ciphertext = encryptor.update(padded_plaintext) + encryptor.finalize()
        
        return iv + ciphertext
    
    def benchmark_aes_performance(self, data_sizes: List[int], key_sizes: List[int]) -> Dict:
        """AESパフォーマンスベンチマーク"""
        
        results = {}
        
        for key_size in key_sizes:
            key = os.urandom(key_size // 8)
            results[f"AES-{key_size}"] = {}
            
            for data_size in data_sizes:
                plaintext = os.urandom(data_size)
                
                # 暗号化ベンチマーク
                start_time = time.time()
                iterations = 1000
                
                for _ in range(iterations):
                    iv = os.urandom(16)
                    cipher = Cipher(algorithms.AES(key), modes.CBC(iv), backend=self.backend)
                    encryptor = cipher.encryptor()
                    ciphertext = encryptor.update(plaintext) + encryptor.finalize()
                
                end_time = time.time()
                total_time = end_time - start_time
                
                results[f"AES-{key_size}"][f"{data_size}_bytes"] = {
                    "total_time": total_time,
                    "ops_per_second": iterations / total_time,
                    "mbps": (data_size * iterations * 8) / (total_time * 1024 * 1024),
                    "latency_ms": (total_time / iterations) * 1000
                }
        
        return results

class AESApplicationScenarios:
    """AES活用シナリオ"""
    
    def __init__(self):
        self.aes_impl = AdvancedAESImplementation()
    
    def database_encryption(self, sensitive_data: Dict[str, str], 
                          master_key: bytes) -> Dict[str, Tuple[bytes, bytes]]:
        """データベース暗号化のシナリオ"""
        
        encrypted_data = {}
        
        for field_name, field_value in sensitive_data.items():
            # フィールドごとに異なるIVを使用
            iv = os.urandom(16)
            
            # データの暗号化
            cipher = Cipher(algorithms.AES(master_key), modes.CBC(iv), backend=default_backend())
            encryptor = cipher.encryptor()
            
            # データをパディング
            padded_data = self.aes_impl._add_pkcs7_padding(field_value.encode(), 16)
            
            # 暗号化
            ciphertext = encryptor.update(padded_data) + encryptor.finalize()
            
            encrypted_data[field_name] = (ciphertext, iv)
        
        return encrypted_data
    
    def cloud_storage_encryption(self, file_path: str, user_key: bytes, 
                               cloud_provider_key: bytes) -> Dict[str, any]:
        """クラウドストレージ暗号化（多層暗号化）"""
        
        # 第1層: ユーザー鍵による暗号化
        user_iv = os.urandom(16)
        user_cipher = Cipher(algorithms.AES(user_key), modes.GCM(user_iv), backend=default_backend())
        user_encryptor = user_cipher.encryptor()
        
        # 第2層: クラウドプロバイダー鍵による暗号化
        provider_iv = os.urandom(16)
        provider_cipher = Cipher(algorithms.AES(cloud_provider_key), modes.GCM(provider_iv), backend=default_backend())
        provider_encryptor = provider_cipher.encryptor()
        
        with open(file_path, 'rb') as f:
            original_data = f.read()
        
        # 第1層暗号化
        user_ciphertext = user_encryptor.update(original_data) + user_encryptor.finalize()
        user_tag = user_encryptor.tag
        
        # 第2層暗号化
        provider_ciphertext = provider_encryptor.update(user_ciphertext) + provider_encryptor.finalize()
        provider_tag = provider_encryptor.tag
        
        return {
            "encrypted_data": provider_ciphertext,
            "user_iv": user_iv,
            "user_tag": user_tag,
            "provider_iv": provider_iv,
            "provider_tag": provider_tag,
            "original_size": len(original_data),
            "encrypted_size": len(provider_ciphertext)
        }
    
    def communication_encryption(self, message: str, shared_key: bytes) -> Dict[str, any]:
        """通信暗号化のシナリオ"""
        
        # タイムスタンプの追加（リプレイ攻撃対策）
        timestamp = int(time.time())
        message_with_timestamp = f"{timestamp}:{message}"
        
        # メッセージの暗号化
        iv = os.urandom(12)  # GCMモード用
        cipher = Cipher(algorithms.AES(shared_key), modes.GCM(iv), backend=default_backend())
        encryptor = cipher.encryptor()
        
        ciphertext = encryptor.update(message_with_timestamp.encode()) + encryptor.finalize()
        tag = encryptor.tag
        
        return {
            "ciphertext": ciphertext,
            "iv": iv,
            "tag": tag,
            "timestamp": timestamp
        }
    
    def verify_communication_message(self, encrypted_message: Dict[str, any], 
                                   shared_key: bytes, max_age_seconds: int = 300) -> Tuple[bool, Optional[str]]:
        """通信メッセージの検証"""
        
        try:
            # 復号化
            cipher = Cipher(
                algorithms.AES(shared_key), 
                modes.GCM(encrypted_message["iv"], encrypted_message["tag"]), 
                backend=default_backend()
            )
            decryptor = cipher.decryptor()
            
            decrypted_data = decryptor.update(encrypted_message["ciphertext"]) + decryptor.finalize()
            message_with_timestamp = decrypted_data.decode()
            
            # タイムスタンプの検証
            timestamp_str, message = message_with_timestamp.split(":", 1)
            message_timestamp = int(timestamp_str)
            current_timestamp = int(time.time())
            
            if current_timestamp - message_timestamp > max_age_seconds:
                return False, "メッセージの有効期限が切れています"
            
            return True, message
            
        except Exception as e:
            return False, f"メッセージの検証に失敗: {str(e)}"
```

### 非対称暗号化の高度な実装

**RSA暗号システムの詳細実装**

```python
"""
RSA暗号システムの包括的実装
"""

import math
import secrets
from typing import Tuple, Optional, Dict, List
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.backends import default_backend

class AdvancedRSAImplementation:
    """高度なRSA実装"""
    
    def __init__(self):
        self.backend = default_backend()
        self.supported_key_sizes = [1024, 2048, 3072, 4096]
        self.recommended_key_size = 3072  # 2024年現在の推奨値
    
    def generate_rsa_keys_with_analysis(self, key_size: int = 3072) -> Dict:
        """RSA鍵生成と分析"""
        
        if key_size not in self.supported_key_sizes:
            raise ValueError(f"サポートされていない鍵長: {key_size}")
        
        # 鍵生成の開始時間
        start_time = time.time()
        
        # RSA鍵ペアの生成
        private_key = rsa.generate_private_key(
            public_exponent=65537,  # フェルマー数F4 = 2^16 + 1
            key_size=key_size,
            backend=self.backend
        )
        
        generation_time = time.time() - start_time
        
        # 公開鍵の取得
        public_key = private_key.public_key()
        
        # RSAパラメータの取得
        private_numbers = private_key.private_numbers()
        public_numbers = private_numbers.public_numbers
        
        # 安全性分析
        security_analysis = self._analyze_rsa_security(private_numbers, key_size)
        
        return {
            "private_key": private_key,
            "public_key": public_key,
            "key_size": key_size,
            "generation_time": generation_time,
            "public_exponent": public_numbers.e,
            "modulus": public_numbers.n,
            "security_analysis": security_analysis,
            "pem_format": {
                "private_key": private_key.private_bytes(
                    encoding=serialization.Encoding.PEM,
                    format=serialization.PrivateFormat.PKCS8,
                    encryption_algorithm=serialization.NoEncryption()
                ),
                "public_key": public_key.public_bytes(
                    encoding=serialization.Encoding.PEM,
                    format=serialization.PublicFormat.SubjectPublicKeyInfo
                )
            }
        }
    
    def _analyze_rsa_security(self, private_numbers, key_size: int) -> Dict:
        """RSA鍵の安全性分析"""
        
        p = private_numbers.p
        q = private_numbers.q
        n = private_numbers.public_numbers.n
        e = private_numbers.public_numbers.e
        d = private_numbers.private_exponent
        
        # 基本的な安全性チェック
        analysis = {
            "key_size_bits": key_size,
            "estimated_security_level": self._estimate_security_level(key_size),
            "factorization_difficulty": self._estimate_factorization_time(key_size),
            "prime_analysis": {
                "p_bits": p.bit_length(),
                "q_bits": q.bit_length(),
                "p_q_difference": abs(p - q),
                "balanced_primes": abs(p.bit_length() - q.bit_length()) <= 2
            },
            "exponent_analysis": {
                "public_exponent": e,
                "is_fermat_number": e == 65537,
                "private_exponent_size": d.bit_length()
            },
            "vulnerabilities": []
        }
        
        # 脆弱性チェック
        if abs(p - q) < (1 << (key_size // 4)):
            analysis["vulnerabilities"].append("素数が近すぎる（フェルマー因数分解法の脅威）")
        
        if d < (n ** 0.25):
            analysis["vulnerabilities"].append("秘密指数が小さすぎる（Wiener攻撃の脅威）")
        
        return analysis
    
    def _estimate_security_level(self, key_size: int) -> str:
        """セキュリティレベルの推定"""
        security_levels = {
            1024: "弱い（非推奨）",
            2048: "中程度（2030年まで）",
            3072: "強い（2030年以降も安全）",
            4096: "非常に強い"
        }
        return security_levels.get(key_size, "不明")
    
    def _estimate_factorization_time(self, key_size: int) -> str:
        """因数分解時間の推定"""
        factorization_times = {
            1024: "数日〜数週間（2024年現在）",
            2048: "数十年〜数百年",
            3072: "数千年以上",
            4096: "数万年以上"
        }
        return factorization_times.get(key_size, "推定困難")
    
    def hybrid_encryption(self, plaintext: bytes, public_key_pem: bytes) -> Dict:
        """ハイブリッド暗号化（RSA + AES）"""
        
        # AES鍵の生成
        aes_key = secrets.token_bytes(32)  # AES-256
        
        # AESによるデータ暗号化
        aes_impl = AdvancedAESImplementation()
        encrypted_data, iv, tag = aes_impl.encrypt_with_aes_gcm(plaintext, aes_key)
        
        # RSAによるAES鍵の暗号化
        public_key = serialization.load_pem_public_key(public_key_pem, backend=self.backend)
        encrypted_aes_key = public_key.encrypt(
            aes_key,
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        
        return {
            "encrypted_data": encrypted_data,
            "encrypted_key": encrypted_aes_key,
            "iv": iv,
            "tag": tag,
            "encryption_method": "RSA-OAEP + AES-256-GCM"
        }
    
    def hybrid_decryption(self, encrypted_package: Dict, private_key_pem: bytes) -> bytes:
        """ハイブリッド復号化"""
        
        # RSAによるAES鍵の復号化
        private_key = serialization.load_pem_private_key(
            private_key_pem, password=None, backend=self.backend
        )
        
        aes_key = private_key.decrypt(
            encrypted_package["encrypted_key"],
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        
        # AESによるデータ復号化
        aes_impl = AdvancedAESImplementation()
        plaintext = aes_impl.decrypt_with_aes_gcm(
            encrypted_package["encrypted_data"],
            aes_key,
            encrypted_package["iv"],
            encrypted_package["tag"]
        )
        
        return plaintext

class EllipticCurveCryptography:
    """楕円曲線暗号"""
    
    def __init__(self):
        self.backend = default_backend()
        self.supported_curves = {
            "P-256": "secp256r1",
            "P-384": "secp384r1", 
            "P-521": "secp521r1",
            "Curve25519": "curve25519"
        }
    
    def generate_ecdsa_keys(self, curve_name: str = "P-256") -> Dict:
        """ECDSA鍵ペアの生成"""
        
        from cryptography.hazmat.primitives.asymmetric import ec
        
        # 曲線の選択
        curves = {
            "P-256": ec.SECP256R1(),
            "P-384": ec.SECP384R1(),
            "P-521": ec.SECP521R1()
        }
        
        if curve_name not in curves:
            raise ValueError(f"サポートされていない曲線: {curve_name}")
        
        # 鍵生成
        private_key = ec.generate_private_key(curves[curve_name], self.backend)
        public_key = private_key.public_key()
        
        return {
            "private_key": private_key,
            "public_key": public_key,
            "curve": curve_name,
            "key_size": private_key.curve.key_size,
            "pem_format": {
                "private_key": private_key.private_bytes(
                    encoding=serialization.Encoding.PEM,
                    format=serialization.PrivateFormat.PKCS8,
                    encryption_algorithm=serialization.NoEncryption()
                ),
                "public_key": public_key.public_bytes(
                    encoding=serialization.Encoding.PEM,
                    format=serialization.PublicFormat.SubjectPublicKeyInfo
                )
            }
        }
    
    def ecdsa_sign(self, message: bytes, private_key_pem: bytes) -> bytes:
        """ECDSA署名の生成"""
        
        from cryptography.hazmat.primitives.asymmetric import ec
        
        # 秘密鍵のロード
        private_key = serialization.load_pem_private_key(
            private_key_pem, password=None, backend=self.backend
        )
        
        # 署名の生成
        signature = private_key.sign(
            message,
            ec.ECDSA(hashes.SHA256())
        )
        
        return signature
    
    def ecdsa_verify(self, message: bytes, signature: bytes, public_key_pem: bytes) -> bool:
        """ECDSA署名の検証"""
        
        from cryptography.hazmat.primitives.asymmetric import ec
        
        try:
            # 公開鍵のロード
            public_key = serialization.load_pem_public_key(public_key_pem, backend=self.backend)
            
            # 署名の検証
            public_key.verify(signature, message, ec.ECDSA(hashes.SHA256()))
            return True
        except Exception:
            return False
    
    def ecdh_key_exchange(self, private_key_pem: bytes, peer_public_key_pem: bytes) -> bytes:
        """ECDH鍵交換"""
        
        from cryptography.hazmat.primitives.asymmetric import ec
        
        # 鍵のロード
        private_key = serialization.load_pem_private_key(
            private_key_pem, password=None, backend=self.backend
        )
        peer_public_key = serialization.load_pem_public_key(peer_public_key_pem, backend=self.backend)
        
        # 共有秘密の計算
        shared_key = private_key.exchange(ec.ECDH(), peer_public_key)
        
        return shared_key
    
    def curve25519_implementation(self) -> Dict:
        """Curve25519の実装例"""
        
        from cryptography.hazmat.primitives.asymmetric import x25519
        
        # X25519鍵ペアの生成
        private_key = x25519.X25519PrivateKey.generate()
        public_key = private_key.public_key()
        
        return {
            "private_key": private_key,
            "public_key": public_key,
            "algorithm": "X25519",
            "security_level": "128ビット相当",
            "performance": "高速（RSAの約10倍）"
        }

### ハッシュ関数の詳細実装と応用

**暗号学的ハッシュ関数の包括的実装**

```python
"""
ハッシュ関数の詳細実装と応用
"""

import hashlib
import hmac
import time
from typing import Dict, List, Tuple, Optional, Union
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.backends import default_backend

class AdvancedHashFunctions:
    """高度なハッシュ関数実装"""
    
    def __init__(self):
        self.backend = default_backend()
        self.algorithms = {
            "SHA-256": hashes.SHA256(),
            "SHA-384": hashes.SHA384(),
            "SHA-512": hashes.SHA512(),
            "SHA-3-256": hashes.SHA3_256(),
            "SHA-3-512": hashes.SHA3_512(),
            "BLAKE2b": hashes.BLAKE2b(64),
            "BLAKE2s": hashes.BLAKE2s(32)
        }
    
    def hash_comparison_analysis(self, data: bytes) -> Dict:
        """ハッシュアルゴリズムの比較分析"""
        
        results = {}
        
        for name, algorithm in self.algorithms.items():
            start_time = time.time()
            
            # ハッシュ値の計算
            digest = hashes.Hash(algorithm, backend=self.backend)
            digest.update(data)
            hash_value = digest.finalize()
            
            end_time = time.time()
            
            results[name] = {
                "hash_value": hash_value.hex(),
                "hash_length": len(hash_value),
                "processing_time": end_time - start_time,
                "throughput_mbps": (len(data) * 8) / ((end_time - start_time) * 1024 * 1024) if (end_time - start_time) > 0 else 0,
                "collision_resistance": self._get_collision_resistance(name),
                "quantum_resistance": self._get_quantum_resistance(name)
            }
        
        return results
    
    def _get_collision_resistance(self, algorithm: str) -> str:
        """衝突耐性の評価"""
        resistance_levels = {
            "SHA-256": "2^128 operations",
            "SHA-384": "2^192 operations", 
            "SHA-512": "2^256 operations",
            "SHA-3-256": "2^128 operations",
            "SHA-3-512": "2^256 operations",
            "BLAKE2b": "2^256 operations",
            "BLAKE2s": "2^128 operations"
        }
        return resistance_levels.get(algorithm, "不明")
    
    def _get_quantum_resistance(self, algorithm: str) -> str:
        """量子コンピュータ耐性の評価"""
        quantum_resistance = {
            "SHA-256": "2^64 operations (Grover's algorithm)",
            "SHA-384": "2^96 operations",
            "SHA-512": "2^128 operations", 
            "SHA-3-256": "2^64 operations",
            "SHA-3-512": "2^128 operations",
            "BLAKE2b": "2^128 operations",
            "BLAKE2s": "2^64 operations"
        }
        return quantum_resistance.get(algorithm, "不明")
    
    def secure_password_hashing(self, password: str, algorithm: str = "argon2") -> Dict:
        """安全なパスワードハッシュ化"""
        
        # ソルトの生成
        salt = secrets.token_bytes(32)
        
        if algorithm == "argon2":
            return self._argon2_hash(password, salt)
        elif algorithm == "scrypt":
            return self._scrypt_hash(password, salt)
        elif algorithm == "pbkdf2":
            return self._pbkdf2_hash(password, salt)
        else:
            raise ValueError(f"サポートされていないアルゴリズム: {algorithm}")
    
    def _argon2_hash(self, password: str, salt: bytes) -> Dict:
        """Argon2による安全なパスワードハッシュ化"""
        
        try:
            import argon2
            
            # Argon2idを使用（メモリハード + データ依存アクセス耐性）
            hasher = argon2.PasswordHasher(
                time_cost=3,        # 時間コスト（反復回数）
                memory_cost=65536,  # メモリコスト（KB）
                parallelism=1,      # 並列度
                hash_len=32,        # ハッシュ長
                salt_len=16         # ソルト長
            )
            
            hash_value = hasher.hash(password, salt=salt)
            
            return {
                "algorithm": "Argon2id",
                "hash": hash_value,
                "salt": salt.hex(),
                "parameters": {
                    "time_cost": 3,
                    "memory_cost": 65536,
                    "parallelism": 1
                },
                "security_level": "非常に高い（GPU攻撃耐性）"
            }
            
        except ImportError:
            # Argon2が利用できない場合はscryptにフォールバック
            return self._scrypt_hash(password, salt)
    
    def _scrypt_hash(self, password: str, salt: bytes) -> Dict:
        """scryptによるパスワードハッシュ化"""
        
        from cryptography.hazmat.primitives.kdf.scrypt import Scrypt
        
        # scryptパラメータ
        n = 16384   # CPU/メモリコスト（2^14）
        r = 8       # ブロックサイズ
        p = 1       # 並列化パラメータ
        
        kdf = Scrypt(
            algorithm=hashes.SHA256(),
            length=32,
            salt=salt,
            n=n,
            r=r,
            p=p,
            backend=self.backend
        )
        
        hash_value = kdf.derive(password.encode())
        
        return {
            "algorithm": "scrypt",
            "hash": hash_value.hex(),
            "salt": salt.hex(),
            "parameters": {"n": n, "r": r, "p": p},
            "security_level": "高い（GPU攻撃耐性）"
        }
    
    def _pbkdf2_hash(self, password: str, salt: bytes) -> Dict:
        """PBKDF2によるパスワードハッシュ化"""
        
        from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
        
        iterations = 100000  # 反復回数
        
        kdf = PBKDF2HMAC(
            algorithm=hashes.SHA256(),
            length=32,
            salt=salt,
            iterations=iterations,
            backend=self.backend
        )
        
        hash_value = kdf.derive(password.encode())
        
        return {
            "algorithm": "PBKDF2-HMAC-SHA256",
            "hash": hash_value.hex(),
            "salt": salt.hex(),
            "parameters": {"iterations": iterations},
            "security_level": "中程度"
        }
    
    def verify_password(self, password: str, stored_hash: Dict) -> bool:
        """パスワードの検証"""
        
        algorithm = stored_hash["algorithm"]
        salt = bytes.fromhex(stored_hash["salt"])
        
        if algorithm == "Argon2id":
            try:
                import argon2
                hasher = argon2.PasswordHasher()
                hasher.verify(stored_hash["hash"], password)
                return True
            except:
                return False
        
        elif algorithm == "scrypt":
            params = stored_hash["parameters"]
            from cryptography.hazmat.primitives.kdf.scrypt import Scrypt
            
            kdf = Scrypt(
                algorithm=hashes.SHA256(),
                length=32,
                salt=salt,
                n=params["n"],
                r=params["r"],
                p=params["p"],
                backend=self.backend
            )
            
            try:
                kdf.verify(password.encode(), bytes.fromhex(stored_hash["hash"]))
                return True
            except:
                return False
        
        elif algorithm == "PBKDF2-HMAC-SHA256":
            params = stored_hash["parameters"]
            from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
            
            kdf = PBKDF2HMAC(
                algorithm=hashes.SHA256(),
                length=32,
                salt=salt,
                iterations=params["iterations"],
                backend=self.backend
            )
            
            try:
                kdf.verify(password.encode(), bytes.fromhex(stored_hash["hash"]))
                return True
            except:
                return False
        
        return False
    
    def merkle_tree_implementation(self, data_blocks: List[bytes]) -> Dict:
        """Merkle Treeの実装"""
        
        if not data_blocks:
            raise ValueError("データブロックが空です")
        
        # 各データブロックのハッシュを計算
        leaves = []
        for block in data_blocks:
            hash_obj = hashes.Hash(hashes.SHA256(), backend=self.backend)
            hash_obj.update(block)
            leaves.append(hash_obj.finalize())
        
        # Merkle Treeの構築
        tree_levels = [leaves]
        current_level = leaves
        
        while len(current_level) > 1:
            next_level = []
            
            # ペアごとにハッシュを計算
            for i in range(0, len(current_level), 2):
                left = current_level[i]
                right = current_level[i + 1] if i + 1 < len(current_level) else left
                
                hash_obj = hashes.Hash(hashes.SHA256(), backend=self.backend)
                hash_obj.update(left + right)
                combined_hash = hash_obj.finalize()
                next_level.append(combined_hash)
            
            tree_levels.append(next_level)
            current_level = next_level
        
        merkle_root = current_level[0]
        
        return {
            "merkle_root": merkle_root.hex(),
            "tree_levels": [[h.hex() for h in level] for level in tree_levels],
            "leaf_count": len(leaves),
            "tree_height": len(tree_levels)
        }
    
    def generate_merkle_proof(self, data_blocks: List[bytes], target_index: int) -> Dict:
        """Merkle Proof の生成"""
        
        if target_index >= len(data_blocks):
            raise ValueError("インデックスが範囲外です")
        
        # Merkle Treeの構築
        merkle_tree = self.merkle_tree_implementation(data_blocks)
        tree_levels = [[bytes.fromhex(h) for h in level] for level in merkle_tree["tree_levels"]]
        
        # プルーフパスの生成
        proof_path = []
        current_index = target_index
        
        for level in tree_levels[:-1]:  # ルートレベルは除外
            # シブリングのインデックスを計算
            sibling_index = current_index + 1 if current_index % 2 == 0 else current_index - 1
            
            if sibling_index < len(level):
                proof_path.append({
                    "hash": level[sibling_index].hex(),
                    "direction": "right" if current_index % 2 == 0 else "left"
                })
            
            current_index = current_index // 2
        
        return {
            "target_index": target_index,
            "target_hash": tree_levels[0][target_index].hex(),
            "proof_path": proof_path,
            "merkle_root": merkle_tree["merkle_root"]
        }
    
    def verify_merkle_proof(self, proof: Dict, target_data: bytes) -> bool:
        """Merkle Proof の検証"""
        
        # ターゲットデータのハッシュを計算
        hash_obj = hashes.Hash(hashes.SHA256(), backend=self.backend)
        hash_obj.update(target_data)
        current_hash = hash_obj.finalize()
        
        # プルーフパスに沿ってハッシュを計算
        for step in proof["proof_path"]:
            sibling_hash = bytes.fromhex(step["hash"])
            
            hash_obj = hashes.Hash(hashes.SHA256(), backend=self.backend)
            
            if step["direction"] == "right":
                hash_obj.update(current_hash + sibling_hash)
            else:
                hash_obj.update(sibling_hash + current_hash)
            
            current_hash = hash_obj.finalize()
        
        # 計算されたルートハッシュと期待値を比較
        return current_hash.hex() == proof["merkle_root"]

class DigitalSignatureFramework:
    """デジタル署名フレームワーク"""
    
    def __init__(self):
        self.backend = default_backend()
        self.hash_algorithms = {
            "SHA-256": hashes.SHA256(),
            "SHA-384": hashes.SHA384(),
            "SHA-512": hashes.SHA512()
        }
    
    def create_multi_signature_scheme(self, signers: List[Dict], threshold: int) -> Dict:
        """マルチシグネチャスキームの作成"""
        
        if threshold > len(signers):
            raise ValueError("閾値が署名者数を超えています")
        
        # 各署名者の公開鍵を収集
        public_keys = []
        for signer in signers:
            public_keys.append(signer["public_key"])
        
        # マルチシグネチャのコンテキストを作成
        multisig_context = {
            "signers": signers,
            "threshold": threshold,
            "public_keys": public_keys,
            "total_signers": len(signers),
            "scheme_type": f"{threshold}-of-{len(signers)} multisig"
        }
        
        return multisig_context
    
    def sign_with_multiple_keys(self, message: bytes, signers: List[Dict], 
                              multisig_context: Dict) -> Dict:
        """複数鍵による署名"""
        
        signatures = []
        
        for signer in signers:
            # 各署名者による署名
            signature = self._create_individual_signature(
                message, 
                signer["private_key"], 
                signer.get("algorithm", "RSA-PSS")
            )
            
            signatures.append({
                "signer_id": signer["id"],
                "signature": signature,
                "public_key": signer["public_key"],
                "timestamp": int(time.time())
            })
        
        return {
            "message_hash": hashlib.sha256(message).hexdigest(),
            "signatures": signatures,
            "multisig_context": multisig_context,
            "valid_signatures": len(signatures),
            "threshold_met": len(signatures) >= multisig_context["threshold"]
        }
    
    def verify_multisignature(self, message: bytes, multisig_data: Dict) -> Dict:
        """マルチシグネチャの検証"""
        
        verification_results = []
        valid_count = 0
        
        for sig_data in multisig_data["signatures"]:
            # 個別署名の検証
            is_valid = self._verify_individual_signature(
                message,
                sig_data["signature"],
                sig_data["public_key"],
                "RSA-PSS"  # デフォルトアルゴリズム
            )
            
            verification_results.append({
                "signer_id": sig_data["signer_id"],
                "valid": is_valid,
                "timestamp": sig_data["timestamp"]
            })
            
            if is_valid:
                valid_count += 1
        
        threshold = multisig_data["multisig_context"]["threshold"]
        
        return {
            "overall_valid": valid_count >= threshold,
            "valid_signatures": valid_count,
            "required_threshold": threshold,
            "individual_results": verification_results,
            "verification_timestamp": int(time.time())
        }
    
    def _create_individual_signature(self, message: bytes, private_key_pem: bytes, 
                                   algorithm: str) -> bytes:
        """個別署名の作成"""
        
        if algorithm == "RSA-PSS":
            private_key = serialization.load_pem_private_key(
                private_key_pem, password=None, backend=self.backend
            )
            
            signature = private_key.sign(
                message,
                padding.PSS(
                    mgf=padding.MGF1(hashes.SHA256()),
                    salt_length=padding.PSS.MAX_LENGTH
                ),
                hashes.SHA256()
            )
            return signature
        
        elif algorithm == "ECDSA":
            from cryptography.hazmat.primitives.asymmetric import ec
            
            private_key = serialization.load_pem_private_key(
                private_key_pem, password=None, backend=self.backend
            )
            
            signature = private_key.sign(message, ec.ECDSA(hashes.SHA256()))
            return signature
        
        else:
            raise ValueError(f"サポートされていない署名アルゴリズム: {algorithm}")
    
    def _verify_individual_signature(self, message: bytes, signature: bytes, 
                                   public_key_pem: bytes, algorithm: str) -> bool:
        """個別署名の検証"""
        
        try:
            if algorithm == "RSA-PSS":
                public_key = serialization.load_pem_public_key(public_key_pem, backend=self.backend)
                
                public_key.verify(
                    signature,
                    message,
                    padding.PSS(
                        mgf=padding.MGF1(hashes.SHA256()),
                        salt_length=padding.PSS.MAX_LENGTH
                    ),
                    hashes.SHA256()
                )
                return True
            
            elif algorithm == "ECDSA":
                from cryptography.hazmat.primitives.asymmetric import ec
                
                public_key = serialization.load_pem_public_key(public_key_pem, backend=self.backend)
                public_key.verify(signature, message, ec.ECDSA(hashes.SHA256()))
                return True
            
            return False
            
        except Exception:
            return False
```

続けて量子耐性暗号、暗号プロトコル、実践的な活用例を作成していきます。 

## 💡 実践的な活用

### 量子耐性暗号への移行戦略

**ポスト量子暗号（Post-Quantum Cryptography）**

量子コンピュータの脅威に対応するため、NIST（米国標準技術研究所）が標準化を進めているポスト量子暗号アルゴリズムの実装例：

```python
"""
量子耐性暗号の実装例
"""

class PostQuantumCryptography:
    """量子耐性暗号システム"""
    
    def __init__(self):
        self.nist_standards = {
            "CRYSTALS-Kyber": "格子ベース公開鍵暗号",
            "CRYSTALS-Dilithium": "格子ベースデジタル署名",
            "FALCON": "格子ベースデジタル署名",
            "SPHINCS+": "ハッシュベース署名"
        }
        self.migration_status = "準備段階"
    
    def kyber_key_encapsulation(self) -> Dict:
        """CRYSTALS-Kyberによる鍵カプセル化"""
        
        # 実装例（実際の本番環境では専用ライブラリを使用）
        return {
            "algorithm": "CRYSTALS-Kyber-768",
            "public_key_size": 1184,
            "ciphertext_size": 1088,
            "shared_secret_size": 32,
            "security_level": "NIST Level 3",
            "quantum_resistance": "高い"
        }
    
    def dilithium_signature(self) -> Dict:
        """CRYSTALS-Dilithiumによるデジタル署名"""
        
        return {
            "algorithm": "CRYSTALS-Dilithium3",
            "public_key_size": 1952,
            "signature_size": 3293,
            "private_key_size": 4000,
            "security_level": "NIST Level 3",
            "quantum_resistance": "高い"
        }
    
    def create_migration_plan(self) -> Dict:
        """量子耐性暗号への移行計画"""
        
        return {
            "phase_1": {
                "period": "2024-2025",
                "actions": [
                    "暗号アジリティフレームワークの導入",
                    "ハイブリッド暗号システムの構築",
                    "量子耐性暗号のテスト環境構築"
                ]
            },
            "phase_2": {
                "period": "2025-2027",
                "actions": [
                    "段階的な量子耐性暗号の導入",
                    "既存システムとの互換性確保",
                    "パフォーマンスの最適化"
                ]
            },
            "phase_3": {
                "period": "2027-2030",
                "actions": [
                    "完全な量子耐性暗号への移行",
                    "旧暗号システムの廃止",
                    "継続的な監視と更新"
                ]
            }
        }

### エンタープライズ暗号管理システム

**統合暗号管理プラットフォーム**

```python
"""
エンタープライズ暗号管理システム
"""

class EnterpriseCryptographyManager:
    """エンタープライズ暗号管理システム"""
    
    def __init__(self):
        self.hsm_integration = True
        self.key_lifecycle_management = True
        self.compliance_frameworks = ["FIPS 140-2", "Common Criteria", "ISO 27001"]
    
    def implement_crypto_agility(self) -> Dict:
        """暗号アジリティの実装"""
        
        return {
            "architecture": {
                "abstraction_layer": "統一暗号API",
                "algorithm_registry": "動的アルゴリズム登録",
                "configuration_management": "中央集権的設定管理",
                "monitoring_system": "リアルタイム監視"
            },
            "supported_algorithms": {
                "current": ["AES-256", "RSA-3072", "ECDSA-P256"],
                "transitional": ["RSA-4096", "ECDSA-P384"],
                "future": ["Kyber-768", "Dilithium3", "SPHINCS+"]
            },
            "migration_capabilities": {
                "algorithm_rollback": "緊急時の旧アルゴリズムへの復帰",
                "gradual_deployment": "段階的な新アルゴリズムの導入",
                "compatibility_mode": "複数アルゴリズムの同時サポート"
            }
        }
    
    def key_management_lifecycle(self) -> Dict:
        """鍵管理ライフサイクル"""
        
        return {
            "generation": {
                "entropy_sources": ["TRNG", "HRNG", "Multiple sources"],
                "key_derivation": ["PBKDF2", "scrypt", "Argon2"],
                "quality_assurance": "統計的ランダム性テスト"
            },
            "distribution": {
                "secure_channels": ["TLS 1.3", "SSH", "専用プロトコル"],
                "key_escrow": "規制要件への対応",
                "multi_party_computation": "分散鍵生成"
            },
            "storage": {
                "hsm_integration": "ハードウェアセキュリティモジュール",
                "key_wrapping": "階層的鍵暗号化",
                "backup_recovery": "災害復旧対応"
            },
            "rotation": {
                "automated_rotation": "定期的な鍵更新",
                "emergency_rotation": "緊急時の鍵交換",
                "backward_compatibility": "旧鍵との互換性"
            },
            "revocation": {
                "certificate_revocation": "PKI証明書の失効",
                "key_blacklisting": "危険な鍵の無効化",
                "audit_trail": "完全な監査証跡"
            }
        }

### 暗号プロトコルの実装

**TLS 1.3の詳細実装**

```python
"""
TLS 1.3プロトコルの実装例
"""

class TLS13Implementation:
    """TLS 1.3プロトコルの実装"""
    
    def __init__(self):
        self.supported_ciphersuites = [
            "TLS_AES_128_GCM_SHA256",
            "TLS_AES_256_GCM_SHA384",
            "TLS_CHACHA20_POLY1305_SHA256"
        ]
        self.supported_curves = ["X25519", "P-256", "P-384"]
    
    def implement_0rtt_resumption(self) -> Dict:
        """0-RTTセッション再開の実装"""
        
        return {
            "benefits": {
                "latency_reduction": "初回接続時間の短縮",
                "performance_improvement": "特にモバイル環境での向上",
                "user_experience": "シームレスな接続体験"
            },
            "security_considerations": {
                "replay_attacks": "リプレイ攻撃の可能性",
                "forward_secrecy": "前方秘匿性の一時的な低下",
                "anti_replay": "アンチリプレイ機構の実装"
            },
            "implementation_guidelines": {
                "psk_validation": "事前共有鍵の厳格な検証",
                "ticket_lifecycle": "チケットの適切な管理",
                "fallback_mechanism": "1-RTTハンドシェイクへの自動切り替え"
            }
        }
    
    def perfect_forward_secrecy(self) -> Dict:
        """完全前方秘匿性の実装"""
        
        return {
            "key_exchange": {
                "ephemeral_keys": "一時的な鍵ペアの生成",
                "ecdhe": "楕円曲線Diffie-Hellman鍵交換",
                "x25519": "Curve25519による高速鍵交換"
            },
            "session_keys": {
                "derivation": "HKDF（HMAC-based Key Derivation Function）",
                "rotation": "定期的な鍵更新",
                "isolation": "セッション間の鍵分離"
            },
            "security_benefits": {
                "long_term_protection": "長期間の秘匿性確保",
                "key_compromise_resistance": "鍵漏洩時の影響最小化",
                "regulatory_compliance": "規制要件への対応"
            }
        }
```

## 🔍 深掘り：プロの視点

### 暗号実装における高度な考慮事項

**サイドチャネル攻撃対策**

```python
"""
サイドチャネル攻撃対策の実装
"""

class SideChannelResistantCrypto:
    """サイドチャネル攻撃耐性暗号実装"""
    
    def __init__(self):
        self.attack_vectors = {
            "timing_attacks": "実行時間の変動を利用",
            "power_analysis": "消費電力の変動を分析",
            "electromagnetic_attacks": "電磁波漏洩の解析",
            "cache_attacks": "キャッシュアクセスパターンの解析"
        }
    
    def constant_time_implementation(self) -> Dict:
        """定数時間実装のガイドライン"""
        
        return {
            "principles": {
                "data_independent_execution": "データに依存しない実行時間",
                "branch_elimination": "条件分岐の除去",
                "constant_memory_access": "一定のメモリアクセスパターン"
            },
            "implementation_techniques": {
                "bit_slicing": "ビットスライシング技法",
                "table_lookup_masking": "テーブル参照のマスキング",
                "conditional_selection": "条件付き選択の安全な実装"
            },
            "verification_methods": {
                "formal_verification": "形式検証によるアプローチ",
                "statistical_testing": "統計的テストによる検証",
                "hardware_analysis": "ハードウェアレベルでの分析"
            }
        }
    
    def power_analysis_countermeasures(self) -> Dict:
        """電力解析攻撃対策"""
        
        return {
            "masking_techniques": {
                "boolean_masking": "ブール演算マスキング",
                "arithmetic_masking": "算術演算マスキング",
                "higher_order_masking": "高次マスキング"
            },
            "randomization_strategies": {
                "operation_randomization": "演算順序のランダム化",
                "dummy_operations": "ダミー演算の挿入",
                "noise_injection": "ノイズの意図的な注入"
            },
            "hardware_countermeasures": {
                "dual_rail_logic": "デュアルレール論理",
                "power_regulators": "電力レギュレータ",
                "faraday_cage": "ファラデーケージの使用"
            }
        }

### 暗号システムの性能最適化

**並列処理とハードウェア最適化**

```python
"""
暗号システムの性能最適化
"""

class CryptographicOptimization:
    """暗号システムの最適化"""
    
    def __init__(self):
        self.optimization_targets = {
            "throughput": "処理速度の向上",
            "latency": "応答時間の短縮",
            "energy_efficiency": "電力効率の改善",
            "memory_usage": "メモリ使用量の最適化"
        }
    
    def aes_ni_optimization(self) -> Dict:
        """AES-NI命令による最適化"""
        
        return {
            "performance_gains": {
                "encryption_speed": "10-15倍の高速化",
                "decryption_speed": "8-12倍の高速化",
                "key_expansion": "5-8倍の高速化"
            },
            "implementation_considerations": {
                "cpu_support": "Intel AES-NI, ARM Crypto Extensions",
                "compiler_optimization": "適切なコンパイラフラグ",
                "fallback_mechanism": "非対応CPUでの代替実装"
            },
            "security_benefits": {
                "side_channel_resistance": "ハードウェアレベルでの対策",
                "constant_time_execution": "定数時間実行の保証",
                "cache_attack_resistance": "キャッシュ攻撃への耐性"
            }
        }
    
    def gpu_acceleration(self) -> Dict:
        """GPU加速による暗号処理"""
        
        return {
            "suitable_algorithms": {
                "hash_functions": "SHA-256, SHA-3, BLAKE2",
                "symmetric_ciphers": "AES（特にCTRモード）",
                "public_key_crypto": "楕円曲線暗号の一部演算"
            },
            "performance_characteristics": {
                "parallel_throughput": "大量データの並列処理",
                "batch_processing": "バッチ処理による効率化",
                "memory_bandwidth": "高メモリ帯域の活用"
            },
            "implementation_challenges": {
                "memory_management": "GPU-CPU間のデータ転送",
                "precision_requirements": "浮動小数点演算の精度",
                "security_considerations": "GPU上での秘密情報の保護"
            }
        }
```

## 📋 まとめとチェックポイント

### 重要ポイントの再確認

**暗号学の基本原則**
- 機密性、完全性、認証、否認防止の4つの基本目標
- 対称暗号化と非対称暗号化の適切な使い分け
- ハッシュ関数の暗号学的性質と応用
- デジタル署名による完全性と認証の確保

**実装における重要な考慮事項**
- 暗号学的に安全な乱数生成の重要性
- 適切な鍵管理ライフサイクルの実装
- サイドチャネル攻撃対策の必要性
- 性能と安全性のバランス

**将来への備え**
- 量子コンピュータ脅威への対応
- 暗号アジリティフレームワークの導入
- 継続的な暗号標準の更新

### 理解度確認のためのセルフチェック項目

**基礎レベル**
- [ ] 対称暗号化と非対称暗号化の違いを説明できる
- [ ] ハッシュ関数の特性と用途を理解している
- [ ] デジタル署名の仕組みを説明できる
- [ ] 基本的な暗号プロトコル（TLS）の概要を理解している

**中級レベル**
- [ ] AES-GCMの認証付き暗号化を実装できる
- [ ] RSA-OAEPによる安全な公開鍵暗号化を実装できる
- [ ] 適切なパスワードハッシュ化（Argon2, scrypt）を実装できる
- [ ] 鍵導出関数（PBKDF2, HKDF）を適切に使用できる

**上級レベル**
- [ ] 楕円曲線暗号の実装と最適化ができる
- [ ] サイドチャネル攻撃対策を考慮した実装ができる
- [ ] 暗号プロトコルの設計と実装ができる
- [ ] 暗号システムの性能分析と最適化ができる

**マスターレベル**
- [ ] 量子耐性暗号の実装と移行計画を立てられる
- [ ] エンタープライズ暗号管理システムを設計できる
- [ ] 暗号アジリティフレームワークを構築できる
- [ ] 暗号システムの包括的セキュリティ評価ができる

## 🔗 関連知識・発展学習

### 必読書籍・技術標準

**基礎理論**
- "Introduction to Modern Cryptography" by Katz & Lindell
- "Cryptography Engineering" by Ferguson, Schneier & Kohno
- "The Handbook of Applied Cryptography" by Menezes, van Oorschot & Vanstone

**実装ガイド**
- NIST SP 800-57: "Recommendation for Key Management"
- FIPS 140-2: "Security Requirements for Cryptographic Modules"
- RFC 8446: "The Transport Layer Security (TLS) Protocol Version 1.3"

**量子耐性暗号**
- NIST Post-Quantum Cryptography Standardization
- "Post-Quantum Cryptography" by Bernstein, Buchmann & Dahmen

### 継続的な学習リソース

**技術コミュニティ**
- IACR (International Association for Cryptologic Research)
- Real World Crypto Conference
- Crypto Forum Research Group (CFRG)

**実践的な学習プラットフォーム**
- CryptoHack: 暗号学の実践的な問題解決
- Cryptopals: 暗号システムの攻撃と防御
- NIST Cryptographic Algorithm Validation Program

**オンライン教育**
- Stanford Cryptography Course (Dan Boneh)
- MIT 6.857 Network and Computer Security
- Coursera: Cryptography Specialization

### 次章への橋渡し

本章で学んだ暗号化とハッシュ化の知識は、次章「セキュアコーディング」で実際のアプリケーション開発における安全な実装技術の基盤となります。また、「セキュリティテストと脆弱性診断」では、暗号実装の脆弱性を発見し、修正する技術を学びます。

**関連する次の学習トピック**
- セキュアコーディング実践における暗号化の適用
- 暗号実装の脆弱性テスト手法
- PKI（公開鍵基盤）の設計と運用
- ゼロトラスト・アーキテクチャにおける暗号化戦略

---

この章で学んだ暗号化とハッシュ化の知識は、現代のデジタル社会において不可欠なセキュリティ技術の基盤です。量子コンピュータ時代の到来に備え、継続的な学習と実践を通じて、常に最新の暗号技術に精通し、安全で信頼性の高いシステムを構築できるエンジニアを目指してください。 