# セキュアコーディング：攻撃を防ぐ安全な実装技術

## 🎯 この章で学ぶこと
- セキュアコーディングの基本原則と設計思想の理解
- 言語別（Python、JavaScript、Java、C++）のセキュア実装パターン
- 脆弱性を作り込まない設計手法とアーキテクチャパターン
- セキュリティバイデザイン（Security by Design）の実践
- 静的解析・動的解析ツールの活用と自動化
- セキュリティテスト駆動開発（Security TDD）の実装
- エンタープライズレベルのセキュアアーキテクチャ設計
- API、データベース、クラウドのセキュア実装技術
- DevSecOps パイプラインの構築と運用
- セキュリティ監査とコンプライアンス対応

## 🤔 なぜ重要なのか

### セキュアコーディングの現実的な必要性

**現代のサイバー攻撃の高度化**

2024年現在、サイバー攻撃は年々高度化・自動化されており、従来の「後付けセキュリティ」では対応が困難になっています。セキュアコーディングは、攻撃者に隙を与えない強固なシステムを構築する唯一の方法です。

```
セキュアコーディングの重要性（2024年現在）
┌─────────────────────────────────────────────────────┐
│ 攻撃の現状                                          │
│ ├── 自動化された脆弱性スキャン: 24時間365日の攻撃    │
│ ├── AIを活用した攻撃手法: 機械学習による脆弱性発見  │
│ ├── サプライチェーン攻撃: 信頼できるソフトウェアの悪用│
│ └── ゼロデイ攻撃: 未知の脆弱性を狙った攻撃        │
│                                                     │
│ 経済的影響                                          │
│ ├── 平均データ侵害コスト: 445万ドル（2024年）      │
│ ├── 企業価値の損失: 平均27%の株価下落              │
│ ├── 復旧時間: 平均287日（発見から復旧まで）        │
│ └── 法的制裁: GDPR最大4%の年間売上高を制裁金       │
│                                                     │
│ 技術的課題                                          │
│ ├── 複雑化するアーキテクチャ: マイクロサービス、クラウド│
│ ├── 開発速度の要求: DevOpsによる高速リリース        │
│ ├── 技術スタックの多様化: 多言語、多フレームワーク  │
│ └── 規制要件の厳格化: コンプライアンス対応の必要性  │
└─────────────────────────────────────────────────────┘
```

### 実際のセキュアコーディング不備による事例

**Log4Shell脆弱性事件（2021年）**
```
原因: セキュアコーディング原則の軽視
├── 入力値検証の不備
│   ├── ユーザー入力の直接処理
│   ├── JNDI Lookup機能の無制限実行
│   └── 信頼境界の設定不備
│
├── 影響範囲の拡大
│   ├── 全世界で数十万のアプリケーションが影響
│   ├── Apache、Microsoft、Appleなど大手企業も被害
│   └── 重要インフラへの攻撃に悪用
│
└── 学習すべき教訓
    ├── デフォルトセキュアの重要性
    ├── 依存関係の継続的な監視
    ├── 最小権限の原則
    └── 多層防御の実装
```

**SolarWinds Orion事件（2020年）**
```
原因: 開発プロセスのセキュリティ不備
├── ビルドシステムの侵害
│   ├── 認証情報の平文保存
│   ├── コードサイニングプロセスの脆弱性
│   └── 異常検知機能の不備
│
├── 影響とその深刻さ
│   ├── 18,000以上の組織が影響
│   ├── 米国政府機関への侵入
│   └── 9か月間の潜伏期間
│
└── セキュアコーディングの重要性
    ├── セキュアな開発環境の構築
    ├── コード整合性の検証
    ├── サプライチェーンセキュリティ
    └── 継続的なセキュリティ監視
```

## 📚 基礎概念の理解

### セキュアコーディングの基本原則

**CIA トライアド + 3つの拡張原則**

```python
"""
セキュアコーディングの基本原則実装
"""

from abc import ABC, abstractmethod
from typing import Dict, List, Optional, Any, Union
from enum import Enum
import hashlib
import hmac
import secrets
import time
import logging
import json
from datetime import datetime, timedelta
from dataclasses import dataclass
from contextlib import contextmanager

class SecurityPrinciple(Enum):
    """セキュリティ原則の定義"""
    CONFIDENTIALITY = "機密性"
    INTEGRITY = "完全性"
    AVAILABILITY = "可用性"
    AUTHENTICITY = "真正性"
    AUTHORIZATION = "認可"
    NON_REPUDIATION = "否認防止"

class ThreatModel(Enum):
    """脅威モデル"""
    SPOOFING = "なりすまし"
    TAMPERING = "改ざん"
    REPUDIATION = "否認"
    INFORMATION_DISCLOSURE = "情報漏洩"
    DENIAL_OF_SERVICE = "サービス拒否"
    ELEVATION_OF_PRIVILEGE = "権限昇格"

@dataclass
class SecurityContext:
    """セキュリティコンテキスト"""
    user_id: str
    session_id: str
    permissions: List[str]
    ip_address: str
    user_agent: str
    timestamp: datetime
    risk_score: float = 0.0
    
    def is_high_risk(self) -> bool:
        """高リスクセッションの判定"""
        return self.risk_score > 0.7

class SecureCodeBase(ABC):
    """セキュアコーディングの基底クラス"""
    
    def __init__(self):
        self.logger = self._setup_secure_logging()
        self.audit_trail = []
        self.security_context = None
    
    def _setup_secure_logging(self) -> logging.Logger:
        """セキュアログシステムの設定"""
        logger = logging.getLogger(self.__class__.__name__)
        logger.setLevel(logging.INFO)
        
        # セキュリティイベント専用のハンドラー
        security_handler = logging.StreamHandler()
        security_formatter = logging.Formatter(
            '%(asctime)s - SECURITY - %(name)s - %(levelname)s - %(message)s'
        )
        security_handler.setFormatter(security_formatter)
        logger.addHandler(security_handler)
        
        return logger
    
    def log_security_event(self, event_type: str, details: Dict[str, Any]):
        """セキュリティイベントの記録"""
        event = {
            "timestamp": datetime.now().isoformat(),
            "event_type": event_type,
            "details": details,
            "session_id": self.security_context.session_id if self.security_context else None,
            "user_id": self.security_context.user_id if self.security_context else None
        }
        
        self.audit_trail.append(event)
        self.logger.info(f"Security Event: {event_type} - {json.dumps(details)}")
    
    @abstractmethod
    def validate_input(self, input_data: Any) -> bool:
        """入力検証の実装"""
        pass
    
    @abstractmethod
    def sanitize_output(self, output_data: Any) -> Any:
        """出力サニタイゼーションの実装"""
        pass
    
    @abstractmethod
    def enforce_authorization(self, required_permission: str) -> bool:
        """認可の実装"""
        pass

class InputValidationFramework:
    """入力検証フレームワーク"""
    
    def __init__(self):
        self.validation_rules = {}
        self.sanitization_rules = {}
    
    def register_validation_rule(self, data_type: str, rule_func: callable):
        """検証ルールの登録"""
        self.validation_rules[data_type] = rule_func
    
    def register_sanitization_rule(self, data_type: str, sanitize_func: callable):
        """サニタイゼーションルールの登録"""
        self.sanitization_rules[data_type] = sanitize_func
    
    def validate_and_sanitize(self, data: Any, data_type: str) -> tuple[bool, Any]:
        """検証とサニタイゼーションの実行"""
        
        # 入力検証
        if data_type in self.validation_rules:
            if not self.validation_rules[data_type](data):
                return False, None
        
        # サニタイゼーション
        if data_type in self.sanitization_rules:
            sanitized_data = self.sanitization_rules[data_type](data)
            return True, sanitized_data
        
        return True, data
    
    def validate_email(self, email: str) -> bool:
        """メールアドレスの検証"""
        import re
        
        # 基本的な形式チェック
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        if not re.match(pattern, email):
            return False
        
        # 長さチェック
        if len(email) > 254:  # RFC 5321の制限
            return False
        
        # 危険な文字の除外
        dangerous_chars = ['<', '>', '"', '\\', '&', '|', ';']
        if any(char in email for char in dangerous_chars):
            return False
        
        return True
    
    def validate_password(self, password: str) -> tuple[bool, List[str]]:
        """パスワードの検証"""
        errors = []
        
        # 長さチェック
        if len(password) < 12:
            errors.append("パスワードは12文字以上である必要があります")
        
        # 複雑さチェック
        if not any(c.isupper() for c in password):
            errors.append("大文字を少なくとも1つ含める必要があります")
        
        if not any(c.islower() for c in password):
            errors.append("小文字を少なくとも1つ含める必要があります")
        
        if not any(c.isdigit() for c in password):
            errors.append("数字を少なくとも1つ含める必要があります")
        
        special_chars = "!@#$%^&*()_+-=[]{}|;:,.<>?"
        if not any(c in special_chars for c in password):
            errors.append("特殊文字を少なくとも1つ含める必要があります")
        
        # 一般的なパスワードチェック
        common_passwords = [
            "password123", "123456789", "qwerty123", "admin123",
            "letmein123", "welcome123", "password1", "123456"
        ]
        
        if password.lower() in [p.lower() for p in common_passwords]:
            errors.append("一般的なパスワードは使用できません")
        
        return len(errors) == 0, errors
    
    def sanitize_html(self, html_content: str) -> str:
        """HTMLの安全なサニタイゼーション"""
        import html
        
        # 基本的なHTMLエスケープ
        escaped = html.escape(html_content)
        
        # 許可されたタグの定義（ホワイトリスト方式）
        allowed_tags = ['p', 'br', 'strong', 'em', 'ul', 'ol', 'li']
        
        # 高度なHTMLサニタイゼーション（実装例）
        # 実際のプロダクションではbleach等の専用ライブラリを使用
        
        return escaped
    
    def validate_sql_input(self, sql_input: str) -> bool:
        """SQL入力の検証"""
        
        # 危険なSQL キーワードの検出
        dangerous_keywords = [
            'DROP', 'DELETE', 'UPDATE', 'INSERT', 'EXEC', 'EXECUTE',
            'UNION', 'SELECT', 'SCRIPT', 'JAVASCRIPT', 'VBSCRIPT',
            'ONLOAD', 'ONERROR', 'ONCLICK', 'ALERT', 'CONFIRM',
            'PROMPT', 'EVAL', 'EXPRESSION', 'IMPORT', 'INCLUDE'
        ]
        
        sql_upper = sql_input.upper()
        
        for keyword in dangerous_keywords:
            if keyword in sql_upper:
                return False
        
        # 特殊文字の検出
        dangerous_chars = [';', '--', '/*', '*/', 'xp_', 'sp_']
        
        for char in dangerous_chars:
            if char in sql_input:
                return False
        
        return True
    
    def validate_file_upload(self, filename: str, file_content: bytes) -> tuple[bool, List[str]]:
        """ファイルアップロードの検証"""
        errors = []
        
        # ファイル名の検証
        if not filename or len(filename) == 0:
            errors.append("ファイル名が空です")
            return False, errors
        
        # 危険な拡張子の検出
        dangerous_extensions = [
            '.exe', '.bat', '.cmd', '.com', '.pif', '.scr', '.vbs', '.js',
            '.jar', '.php', '.asp', '.aspx', '.jsp', '.py', '.pl', '.rb',
            '.sh', '.ps1', '.dll', '.msi', '.deb', '.rpm'
        ]
        
        file_ext = filename.lower().split('.')[-1]
        if f'.{file_ext}' in dangerous_extensions:
            errors.append(f"危険な拡張子です: {file_ext}")
        
        # ファイルサイズの検証
        max_size = 10 * 1024 * 1024  # 10MB
        if len(file_content) > max_size:
            errors.append(f"ファイルサイズが大きすぎます: {len(file_content)} bytes")
        
        # マジックバイトの検証
        if self._is_executable_file(file_content):
            errors.append("実行可能ファイルはアップロードできません")
        
        return len(errors) == 0, errors
    
    def _is_executable_file(self, file_content: bytes) -> bool:
        """実行可能ファイルの検出"""
        
        # 実行可能ファイルのマジックバイト
        executable_signatures = [
            b'MZ',      # Windows PE
            b'\x7fELF', # Linux ELF
            b'\xca\xfe\xba\xbe',  # Java class
            b'PK\x03\x04',        # ZIP (潜在的な実行可能ファイル)
        ]
        
        for signature in executable_signatures:
            if file_content.startswith(signature):
                return True
        
        return False

class AuthorizationFramework:
    """認可フレームワーク"""
    
    def __init__(self):
        self.roles = {}
        self.permissions = {}
        self.resource_policies = {}
    
    def define_role(self, role_name: str, permissions: List[str]):
        """ロールの定義"""
        self.roles[role_name] = permissions
    
    def define_permission(self, permission_name: str, resource_pattern: str):
        """権限の定義"""
        self.permissions[permission_name] = resource_pattern
    
    def check_permission(self, user_roles: List[str], required_permission: str, 
                        resource_id: str = None) -> bool:
        """権限チェック"""
        
        # ユーザーが持つ全ての権限を取得
        user_permissions = []
        for role in user_roles:
            if role in self.roles:
                user_permissions.extend(self.roles[role])
        
        # 必要な権限をチェック
        if required_permission not in user_permissions:
            return False
        
        # リソースレベルの権限チェック
        if resource_id and required_permission in self.permissions:
            resource_pattern = self.permissions[required_permission]
            if not self._match_resource_pattern(resource_pattern, resource_id):
                return False
        
        return True
    
    def _match_resource_pattern(self, pattern: str, resource_id: str) -> bool:
        """リソースパターンのマッチング"""
        import re
        
        # 簡単なパターンマッチング実装
        # 実際のプロダクションではより高度なポリシーエンジンを使用
        
        if pattern == "*":
            return True
        
        pattern_regex = pattern.replace("*", ".*")
        return bool(re.match(pattern_regex, resource_id))

class SecureSessionManager:
    """セキュアセッション管理"""
    
    def __init__(self):
        self.sessions = {}
        self.session_timeout = timedelta(minutes=30)
        self.max_sessions_per_user = 5
    
    def create_session(self, user_id: str, user_agent: str, ip_address: str) -> str:
        """セッションの作成"""
        
        # 既存セッションの数をチェック
        user_sessions = [s for s in self.sessions.values() if s['user_id'] == user_id]
        if len(user_sessions) >= self.max_sessions_per_user:
            # 最も古いセッションを削除
            oldest_session = min(user_sessions, key=lambda x: x['created_at'])
            del self.sessions[oldest_session['session_id']]
        
        # 新しいセッションID の生成
        session_id = secrets.token_urlsafe(32)
        
        # セッション情報の保存
        self.sessions[session_id] = {
            'session_id': session_id,
            'user_id': user_id,
            'user_agent': user_agent,
            'ip_address': ip_address,
            'created_at': datetime.now(),
            'last_activity': datetime.now(),
            'is_active': True
        }
        
        return session_id
    
    def validate_session(self, session_id: str, user_agent: str, ip_address: str) -> bool:
        """セッションの検証"""
        
        if session_id not in self.sessions:
            return False
        
        session = self.sessions[session_id]
        
        # セッションの有効性チェック
        if not session['is_active']:
            return False
        
        # タイムアウトチェック
        if datetime.now() - session['last_activity'] > self.session_timeout:
            session['is_active'] = False
            return False
        
        # セッションハイジャック対策
        if session['user_agent'] != user_agent:
            session['is_active'] = False
            return False
        
        # IP アドレスの変更チェック（厳密な場合）
        if session['ip_address'] != ip_address:
            # IP アドレスが変更された場合の処理
            # 実際のプロダクションでは、地理的位置やISPを考慮した柔軟な判定を行う
            pass
        
        # 最終活動時刻の更新
        session['last_activity'] = datetime.now()
        
        return True
    
    def invalidate_session(self, session_id: str):
        """セッションの無効化"""
        if session_id in self.sessions:
            self.sessions[session_id]['is_active'] = False
    
    def cleanup_expired_sessions(self):
        """期限切れセッションのクリーンアップ"""
        current_time = datetime.now()
        expired_sessions = [
            sid for sid, session in self.sessions.items()
            if current_time - session['last_activity'] > self.session_timeout
        ]
        
        for session_id in expired_sessions:
            del self.sessions[session_id]

class SecureDataProcessor:
    """セキュアデータ処理"""
    
    def __init__(self):
        self.sensitive_data_patterns = [
            r'\b\d{4}[- ]?\d{4}[- ]?\d{4}[- ]?\d{4}\b',  # クレジットカード
            r'\b\d{3}-\d{2}-\d{4}\b',  # SSN
            r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b',  # メール
            r'\b\d{3}-\d{3}-\d{4}\b',  # 電話番号
        ]
    
    def detect_sensitive_data(self, text: str) -> List[Dict[str, str]]:
        """機密データの検出"""
        import re
        
        detected_patterns = []
        
        for pattern in self.sensitive_data_patterns:
            matches = re.finditer(pattern, text)
            for match in matches:
                detected_patterns.append({
                    'type': self._classify_pattern(pattern),
                    'value': match.group(),
                    'start': match.start(),
                    'end': match.end()
                })
        
        return detected_patterns
    
    def _classify_pattern(self, pattern: str) -> str:
        """パターンの分類"""
        if 'credit' in pattern or r'\d{4}[- ]?\d{4}' in pattern:
            return 'credit_card'
        elif 'ssn' in pattern or r'\d{3}-\d{2}-\d{4}' in pattern:
            return 'ssn'
        elif '@' in pattern:
            return 'email'
        elif r'\d{3}-\d{3}-\d{4}' in pattern:
            return 'phone'
        else:
            return 'unknown'
    
    def mask_sensitive_data(self, text: str) -> str:
        """機密データのマスキング"""
        import re
        
        # クレジットカード番号のマスキング
        text = re.sub(r'\b(\d{4})[- ]?(\d{4})[- ]?(\d{4})[- ]?(\d{4})\b', 
                     r'\1-****-****-\4', text)
        
        # SSNのマスキング
        text = re.sub(r'\b(\d{3})-(\d{2})-(\d{4})\b', r'***-**-\3', text)
        
        # メールアドレスのマスキング
        text = re.sub(r'\b([A-Za-z0-9._%+-]+)@([A-Za-z0-9.-]+\.[A-Z|a-z]{2,})\b', 
                     r'****@\2', text)
        
        return text
    
    def encrypt_sensitive_fields(self, data: Dict[str, Any], 
                               encryption_key: bytes) -> Dict[str, Any]:
        """機密フィールドの暗号化"""
        
        # 前章で学んだ暗号化技術を活用
        from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
        from cryptography.hazmat.backends import default_backend
        import os
        
        sensitive_fields = ['password', 'ssn', 'credit_card', 'secret']
        encrypted_data = data.copy()
        
        for field_name, field_value in data.items():
            if any(sensitive_field in field_name.lower() for sensitive_field in sensitive_fields):
                # AES-GCM による暗号化
                iv = os.urandom(12)
                cipher = Cipher(algorithms.AES(encryption_key), modes.GCM(iv), backend=default_backend())
                encryptor = cipher.encryptor()
                
                ciphertext = encryptor.update(str(field_value).encode()) + encryptor.finalize()
                
                encrypted_data[field_name] = {
                    'encrypted': True,
                    'ciphertext': ciphertext.hex(),
                    'iv': iv.hex(),
                    'tag': encryptor.tag.hex()
                }
        
        return encrypted_data

class SecureErrorHandler:
    """セキュアエラーハンドリング"""
    
    def __init__(self):
        self.error_logger = logging.getLogger("SecureErrorHandler")
        self.error_patterns = {}
    
    def register_error_pattern(self, error_type: str, user_message: str, 
                             log_details: bool = True):
        """エラーパターンの登録"""
        self.error_patterns[error_type] = {
            'user_message': user_message,
            'log_details': log_details
        }
    
    def handle_error(self, error: Exception, context: Dict[str, Any] = None) -> str:
        """安全なエラーハンドリング"""
        
        error_type = type(error).__name__
        
        # 詳細なエラー情報をログに記録
        if context:
            self.error_logger.error(f"Error occurred: {error_type} - {str(error)}, Context: {context}")
        else:
            self.error_logger.error(f"Error occurred: {error_type} - {str(error)}")
        
        # ユーザーには安全なメッセージを返す
        if error_type in self.error_patterns:
            return self.error_patterns[error_type]['user_message']
        else:
            # デフォルトの安全なメッセージ
            return "申し訳ございません。システムエラーが発生しました。サポートにお問い合わせください。"
    
    def is_security_related_error(self, error: Exception) -> bool:
        """セキュリティ関連エラーの判定"""
        
        security_error_types = [
            'AuthenticationError',
            'AuthorizationError',
            'ValidationError',
            'SecurityViolationError',
            'SuspiciousActivityError'
        ]
        
        return type(error).__name__ in security_error_types
    
    def handle_security_error(self, error: Exception, context: Dict[str, Any]):
        """セキュリティエラーの特別な処理"""
        
        # セキュリティインシデントとしてログ記録
        self.error_logger.critical(f"SECURITY INCIDENT: {type(error).__name__} - {str(error)}")
        
        # 追加のセキュリティ対策を実行
        if context and 'user_id' in context:
            # ユーザーアカウントの一時的な制限
            self._apply_security_restriction(context['user_id'])
        
        if context and 'ip_address' in context:
            # IP アドレスの監視リストへの追加
            self._add_to_monitoring_list(context['ip_address'])
    
    def _apply_security_restriction(self, user_id: str):
        """セキュリティ制限の適用"""
        # 実装例: ユーザーアカウントの一時的な制限
        pass
    
    def _add_to_monitoring_list(self, ip_address: str):
        """監視リストへの追加"""
        # 実装例: 疑わしいIP アドレスの監視
        pass

## 💡 実践的な活用

### 言語別セキュア実装パターン

**Python セキュア実装**

```python
"""
Python セキュア実装パターン
"""

import os
import secrets
import hashlib
import hmac
from typing import Dict, List, Optional, Any
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC
import sqlite3
import logging
from datetime import datetime, timedelta
import jwt
import bcrypt

class PythonSecureCoding:
    """Python セキュアコーディング実装"""
    
    def __init__(self):
        self.logger = self._setup_logging()
        self.encryption_key = self._generate_encryption_key()
    
    def _setup_logging(self) -> logging.Logger:
        """セキュアログの設定"""
        logger = logging.getLogger("PythonSecure")
        logger.setLevel(logging.INFO)
        
        # ログファイルの安全な設定
        log_file = os.path.join("/var/log/secure", "python_security.log")
        os.makedirs(os.path.dirname(log_file), exist_ok=True)
        
        # ファイルパーミッションの設定（読み書き権限を制限）
        handler = logging.FileHandler(log_file, mode='a')
        os.chmod(log_file, 0o600)  # 所有者のみ読み書き可能
        
        formatter = logging.Formatter(
            '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
        )
        handler.setFormatter(formatter)
        logger.addHandler(handler)
        
        return logger
    
    def secure_password_handling(self, password: str) -> str:
        """安全なパスワード処理"""
        
        # bcryptを使用したパスワードハッシュ化
        salt = bcrypt.gensalt(rounds=12)  # 2024年推奨値
        hashed = bcrypt.hashpw(password.encode('utf-8'), salt)
        
        # ログに機密情報を記録しない
        self.logger.info("Password hashed successfully")
        
        return hashed.decode('utf-8')
    
    def verify_password(self, password: str, hashed: str) -> bool:
        """パスワード検証"""
        try:
            return bcrypt.checkpw(password.encode('utf-8'), hashed.encode('utf-8'))
        except Exception as e:
            self.logger.error(f"Password verification failed: {type(e).__name__}")
            return False
    
    def secure_sql_query(self, user_id: int, table_name: str) -> List[Dict]:
        """安全なSQL クエリ実行"""
        
        # SQLインジェクション対策：パラメータ化クエリ
        # テーブル名の検証（ホワイトリスト方式）
        allowed_tables = ['users', 'products', 'orders', 'categories']
        if table_name not in allowed_tables:
            raise ValueError(f"Invalid table name: {table_name}")
        
        # データベース接続の安全な設定
        conn = sqlite3.connect(
            'secure_app.db',
            isolation_level='DEFERRED',  # 適切な分離レベル
            check_same_thread=False
        )
        
        try:
            cursor = conn.cursor()
            
            # パラメータ化クエリの使用
            query = f"SELECT * FROM {table_name} WHERE user_id = ? AND deleted_at IS NULL"
            cursor.execute(query, (user_id,))
            
            results = cursor.fetchall()
            
            # 結果をディクショナリ形式に変換
            columns = [description[0] for description in cursor.description]
            return [dict(zip(columns, row)) for row in results]
            
        except Exception as e:
            self.logger.error(f"Database query failed: {e}")
            raise
        finally:
            conn.close()
    
    def secure_file_handling(self, filename: str, content: bytes) -> bool:
        """安全なファイル処理"""
        
        # ファイル名のサニタイゼーション
        safe_filename = self._sanitize_filename(filename)
        
        # パストラバーサル攻撃対策
        safe_path = os.path.join("/var/uploads", safe_filename)
        safe_path = os.path.abspath(safe_path)
        
        if not safe_path.startswith("/var/uploads/"):
            raise ValueError("Invalid file path")
        
        # ファイルサイズ制限
        max_size = 10 * 1024 * 1024  # 10MB
        if len(content) > max_size:
            raise ValueError("File too large")
        
        # 安全なファイル書き込み
        try:
            with open(safe_path, 'wb') as f:
                f.write(content)
            
            # ファイルパーミッションの設定
            os.chmod(safe_path, 0o644)
            
            self.logger.info(f"File saved safely: {safe_filename}")
            return True
            
        except Exception as e:
            self.logger.error(f"File save failed: {e}")
            return False
    
    def _sanitize_filename(self, filename: str) -> str:
        """ファイル名のサニタイゼーション"""
        import re
        
        # 危険な文字の除去
        filename = re.sub(r'[^\w\-_\.]', '', filename)
        
        # 拡張子の検証
        allowed_extensions = ['.jpg', '.png', '.pdf', '.txt', '.docx']
        file_ext = os.path.splitext(filename)[1].lower()
        
        if file_ext not in allowed_extensions:
            raise ValueError(f"File extension not allowed: {file_ext}")
        
        return filename
    
    def secure_jwt_handling(self, user_id: str, permissions: List[str]) -> str:
        """安全なJWT トークン処理"""
        
        # JWTペイロードの作成
        payload = {
            'user_id': user_id,
            'permissions': permissions,
            'iat': datetime.utcnow(),
            'exp': datetime.utcnow() + timedelta(hours=1),  # 1時間で期限切れ
            'jti': secrets.token_urlsafe(16)  # JWT ID（リプレイ攻撃対策）
        }
        
        # 安全な署名アルゴリズムの使用
        secret_key = os.environ.get('JWT_SECRET_KEY')
        if not secret_key:
            raise ValueError("JWT secret key not configured")
        
        token = jwt.encode(payload, secret_key, algorithm='HS256')
        return token
    
    def verify_jwt_token(self, token: str) -> Optional[Dict]:
        """JWT トークンの検証"""
        try:
            secret_key = os.environ.get('JWT_SECRET_KEY')
            payload = jwt.decode(token, secret_key, algorithms=['HS256'])
            
            # 追加の検証
            if 'jti' not in payload:
                raise jwt.InvalidTokenError("Missing JTI")
            
            return payload
            
        except jwt.ExpiredSignatureError:
            self.logger.warning("JWT token expired")
            return None
        except jwt.InvalidTokenError as e:
            self.logger.warning(f"Invalid JWT token: {e}")
            return None
    
    def _generate_encryption_key(self) -> bytes:
        """暗号化キーの生成"""
        password = os.environ.get('ENCRYPTION_PASSWORD', 'default-password').encode()
        salt = os.environ.get('ENCRYPTION_SALT', 'default-salt').encode()
        
        kdf = PBKDF2HMAC(
            algorithm=hashes.SHA256(),
            length=32,
            salt=salt,
            iterations=100000,
        )
        return kdf.derive(password)

class JavaScriptSecureCoding:
    """JavaScript セキュアコーディングパターン"""
    
    def __init__(self):
        self.validation_rules = {}
        self.sanitization_rules = {}
    
    @staticmethod
    def javascript_secure_patterns():
        """JavaScript セキュアパターンのガイド"""
        return """
// JavaScript セキュアコーディングパターン

// 1. XSS対策：DOMContentLoaded後の安全な操作
document.addEventListener('DOMContentLoaded', function() {
    // 直接的なinnerHTMLの使用を避ける
    const userInput = document.getElementById('user-input').value;
    
    // 安全：textContentを使用
    document.getElementById('output').textContent = userInput;
    
    // 危険：innerHTML の直接使用
    // document.getElementById('output').innerHTML = userInput;
});

// 2. CSRFトークンの実装
class CSRFProtection {
    constructor() {
        this.token = this.generateToken();
        this.setMetaTag();
    }
    
    generateToken() {
        return Array.from(crypto.getRandomValues(new Uint8Array(32)))
            .map(b => b.toString(16).padStart(2, '0'))
            .join('');
    }
    
    setMetaTag() {
        const meta = document.createElement('meta');
        meta.name = 'csrf-token';
        meta.content = this.token;
        document.head.appendChild(meta);
    }
    
    getToken() {
        return document.querySelector('meta[name="csrf-token"]').content;
    }
    
    // AJAX リクエストでの使用
    makeSecureRequest(url, data) {
        return fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'X-CSRF-Token': this.getToken()
            },
            body: JSON.stringify(data)
        });
    }
}

// 3. 入力検証とサニタイゼーション
class InputValidator {
    constructor() {
        this.patterns = {
            email: /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/,
            phone: /^\\d{3}-\\d{3}-\\d{4}$/,
            username: /^[a-zA-Z0-9_]{3,20}$/
        };
    }
    
    validate(type, value) {
        if (!this.patterns[type]) {
            throw new Error(`Unknown validation type: ${type}`);
        }
        return this.patterns[type].test(value);
    }
    
    sanitizeHTML(str) {
        const div = document.createElement('div');
        div.textContent = str;
        return div.innerHTML;
    }
    
    escapeRegExp(string) {
        return string.replace(/[.*+?^${}()|[\\]\\\\]/g, '\\\\$&');
    }
}

// 4. セキュアなローカルストレージ使用
class SecureStorage {
    constructor(encryptionKey) {
        this.key = encryptionKey;
    }
    
    async setItem(key, value) {
        try {
            const encrypted = await this.encrypt(JSON.stringify(value));
            localStorage.setItem(key, encrypted);
        } catch (error) {
            console.error('Storage encryption failed:', error);
        }
    }
    
    async getItem(key) {
        try {
            const encrypted = localStorage.getItem(key);
            if (!encrypted) return null;
            
            const decrypted = await this.decrypt(encrypted);
            return JSON.parse(decrypted);
        } catch (error) {
            console.error('Storage decryption failed:', error);
            return null;
        }
    }
    
    // Web Crypto API を使用した暗号化
    async encrypt(text) {
        const encoder = new TextEncoder();
        const data = encoder.encode(text);
        
        const iv = crypto.getRandomValues(new Uint8Array(12));
        const encrypted = await crypto.subtle.encrypt(
            { name: 'AES-GCM', iv: iv },
            this.key,
            data
        );
        
        return btoa(JSON.stringify({
            iv: Array.from(iv),
            data: Array.from(new Uint8Array(encrypted))
        }));
    }
    
    async decrypt(encryptedData) {
        const { iv, data } = JSON.parse(atob(encryptedData));
        
        const decrypted = await crypto.subtle.decrypt(
            { name: 'AES-GCM', iv: new Uint8Array(iv) },
            this.key,
            new Uint8Array(data)
        );
        
        return new TextDecoder().decode(decrypted);
    }
}

// 5. Content Security Policy の実装支援
class CSPHelper {
    constructor() {
        this.violations = [];
        this.setupViolationReporting();
    }
    
    setupViolationReporting() {
        document.addEventListener('securitypolicyviolation', (e) => {
            this.violations.push({
                timestamp: new Date().toISOString(),
                blockedURI: e.blockedURI,
                violatedDirective: e.violatedDirective,
                originalPolicy: e.originalPolicy
            });
            
            // セキュリティ違反をサーバーに報告
            this.reportViolation(e);
        });
    }
    
    reportViolation(violationEvent) {
        fetch('/api/csp-violation', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                blockedURI: violationEvent.blockedURI,
                violatedDirective: violationEvent.violatedDirective,
                documentURI: violationEvent.documentURI
            })
        }).catch(console.error);
    }
}

// 使用例
const csrfProtection = new CSRFProtection();
const validator = new InputValidator();
const cspHelper = new CSPHelper();

// セキュアなフォーム送信
document.getElementById('secure-form').addEventListener('submit', async function(e) {
    e.preventDefault();
    
    const email = document.getElementById('email').value;
    const username = document.getElementById('username').value;
    
    // 入力検証
    if (!validator.validate('email', email)) {
        alert('Invalid email format');
        return;
    }
    
    if (!validator.validate('username', username)) {
        alert('Invalid username format');
        return;
    }
    
    // セキュアなリクエスト送信
    try {
        const response = await csrfProtection.makeSecureRequest('/api/register', {
            email: email,
            username: username
        });
        
        if (response.ok) {
            console.log('Registration successful');
        }
    } catch (error) {
        console.error('Registration failed:', error);
    }
});
        """

### APIセキュリティの包括的実装

**RESTful API セキュリティフレームワーク**

```python
"""
APIセキュリティの包括的実装
"""

from flask import Flask, request, jsonify, g
from functools import wraps
import jwt
import time
import hashlib
import hmac
from typing import Dict, List, Optional, Callable
import re
from datetime import datetime, timedelta
import redis
from dataclasses import dataclass

@dataclass
class APISecurityConfig:
    """APIセキュリティ設定"""
    rate_limit_requests: int = 100
    rate_limit_window: int = 3600  # 1時間
    jwt_secret: str = "your-secret-key"
    jwt_expiry: int = 3600  # 1時間
    api_key_header: str = "X-API-Key"
    cors_origins: List[str] = None
    require_https: bool = True

class APISecurityFramework:
    """APIセキュリティフレームワーク"""
    
    def __init__(self, config: APISecurityConfig):
        self.config = config
        self.redis_client = redis.Redis(host='localhost', port=6379, db=0)
        self.blocked_ips = set()
        self.api_keys = {}  # 実際の実装ではデータベースに保存
        self.audit_log = []
    
    def rate_limiting(self, key_func: Callable = None):
        """レート制限デコレータ"""
        def decorator(f):
            @wraps(f)
            def decorated_function(*args, **kwargs):
                # クライアント識別
                if key_func:
                    client_key = key_func()
                else:
                    client_key = request.remote_addr
                
                # レート制限チェック
                rate_key = f"rate_limit:{client_key}"
                current_requests = self.redis_client.get(rate_key)
                
                if current_requests is None:
                    # 新しい時間窓の開始
                    self.redis_client.setex(
                        rate_key, 
                        self.config.rate_limit_window, 
                        1
                    )
                else:
                    current_count = int(current_requests)
                    if current_count >= self.config.rate_limit_requests:
                        self._log_security_event("RATE_LIMIT_EXCEEDED", {
                            "client": client_key,
                            "endpoint": request.endpoint,
                            "count": current_count
                        })
                        return jsonify({
                            "error": "Rate limit exceeded",
                            "retry_after": self.redis_client.ttl(rate_key)
                        }), 429
                    
                    self.redis_client.incr(rate_key)
                
                return f(*args, **kwargs)
            return decorated_function
        return decorator
    
    def require_api_key(self, f):
        """API キー認証デコレータ"""
        @wraps(f)
        def decorated_function(*args, **kwargs):
            api_key = request.headers.get(self.config.api_key_header)
            
            if not api_key:
                self._log_security_event("MISSING_API_KEY", {
                    "endpoint": request.endpoint,
                    "ip": request.remote_addr
                })
                return jsonify({"error": "API key required"}), 401
            
            # API キーの検証
            if not self._validate_api_key(api_key):
                self._log_security_event("INVALID_API_KEY", {
                    "endpoint": request.endpoint,
                    "ip": request.remote_addr,
                    "api_key_prefix": api_key[:8] + "..."
                })
                return jsonify({"error": "Invalid API key"}), 401
            
            # API キー情報をコンテキストに保存
            g.api_key_info = self.api_keys.get(api_key)
            
            return f(*args, **kwargs)
        return decorated_function
    
    def require_jwt(self, f):
        """JWT認証デコレータ"""
        @wraps(f)
        def decorated_function(*args, **kwargs):
            token = None
            auth_header = request.headers.get('Authorization')
            
            if auth_header:
                try:
                    token = auth_header.split(' ')[1]  # "Bearer <token>"
                except IndexError:
                    pass
            
            if not token:
                return jsonify({"error": "Token is missing"}), 401
            
            try:
                # JWT トークンの検証
                payload = jwt.decode(
                    token, 
                    self.config.jwt_secret, 
                    algorithms=['HS256']
                )
                g.current_user = payload
                
                # トークンのブラックリストチェック
                if self._is_token_blacklisted(token):
                    return jsonify({"error": "Token is blacklisted"}), 401
                
            except jwt.ExpiredSignatureError:
                self._log_security_event("EXPIRED_JWT", {
                    "endpoint": request.endpoint,
                    "ip": request.remote_addr
                })
                return jsonify({"error": "Token has expired"}), 401
            except jwt.InvalidTokenError:
                self._log_security_event("INVALID_JWT", {
                    "endpoint": request.endpoint,
                    "ip": request.remote_addr
                })
                return jsonify({"error": "Token is invalid"}), 401
            
            return f(*args, **kwargs)
        return decorated_function
    
    def input_validation(self, schema: Dict):
        """入力検証デコレータ"""
        def decorator(f):
            @wraps(f)
            def decorated_function(*args, **kwargs):
                data = request.get_json()
                
                # データの存在チェック
                if not data:
                    return jsonify({"error": "No JSON data provided"}), 400
                
                # スキーマ検証
                validation_errors = self._validate_schema(data, schema)
                if validation_errors:
                    self._log_security_event("INPUT_VALIDATION_FAILED", {
                        "endpoint": request.endpoint,
                        "errors": validation_errors
                    })
                    return jsonify({
                        "error": "Validation failed",
                        "details": validation_errors
                    }), 400
                
                # サニタイズされたデータをコンテキストに保存
                g.validated_data = self._sanitize_data(data)
                
                return f(*args, **kwargs)
            return decorated_function
        return decorator
    
    def require_https(self, f):
        """HTTPS強制デコレータ"""
        @wraps(f)
        def decorated_function(*args, **kwargs):
            if self.config.require_https and not request.is_secure:
                self._log_security_event("HTTP_ACCESS_ATTEMPT", {
                    "endpoint": request.endpoint,
                    "ip": request.remote_addr
                })
                return jsonify({
                    "error": "HTTPS required"
                }), 400
            
            return f(*args, **kwargs)
        return decorated_function
    
    def cors_protection(self, f):
        """CORS保護デコレータ"""
        @wraps(f)
        def decorated_function(*args, **kwargs):
            origin = request.headers.get('Origin')
            
            if origin and self.config.cors_origins:
                if origin not in self.config.cors_origins:
                    self._log_security_event("CORS_VIOLATION", {
                        "endpoint": request.endpoint,
                        "origin": origin,
                        "ip": request.remote_addr
                    })
                    return jsonify({"error": "CORS policy violation"}), 403
            
            response = f(*args, **kwargs)
            
            # CORS ヘッダーの設定
            if isinstance(response, tuple):
                response_data, status_code = response
                response = jsonify(response_data)
                response.status_code = status_code
            
            if origin and self.config.cors_origins and origin in self.config.cors_origins:
                response.headers['Access-Control-Allow-Origin'] = origin
                response.headers['Access-Control-Allow-Methods'] = 'GET, POST, PUT, DELETE, OPTIONS'
                response.headers['Access-Control-Allow-Headers'] = 'Content-Type, Authorization, X-API-Key'
            
            return response
        return decorated_function
    
    def sql_injection_protection(self, f):
        """SQLインジェクション保護デコレータ"""
        @wraps(f)
        def decorated_function(*args, **kwargs):
            # リクエストデータのSQLインジェクションパターンチェック
            data = request.get_json() or {}
            query_params = dict(request.args)
            
            all_data = {**data, **query_params}
            
            if self._detect_sql_injection(all_data):
                self._log_security_event("SQL_INJECTION_ATTEMPT", {
                    "endpoint": request.endpoint,
                    "ip": request.remote_addr,
                    "data_keys": list(all_data.keys())
                })
                return jsonify({"error": "Malicious input detected"}), 400
            
            return f(*args, **kwargs)
        return decorated_function
    
    def _validate_api_key(self, api_key: str) -> bool:
        """API キーの検証"""
        return api_key in self.api_keys
    
    def _is_token_blacklisted(self, token: str) -> bool:
        """トークンのブラックリストチェック"""
        blacklist_key = f"blacklist:{hashlib.sha256(token.encode()).hexdigest()}"
        return self.redis_client.exists(blacklist_key)
    
    def _validate_schema(self, data: Dict, schema: Dict) -> List[str]:
        """スキーマ検証"""
        errors = []
        
        for field, rules in schema.items():
            value = data.get(field)
            
            # 必須フィールドチェック
            if rules.get('required') and value is None:
                errors.append(f"Field '{field}' is required")
                continue
            
            if value is not None:
                # 型チェック
                expected_type = rules.get('type')
                if expected_type and not isinstance(value, expected_type):
                    errors.append(f"Field '{field}' must be of type {expected_type.__name__}")
                
                # 長さチェック
                if 'max_length' in rules and hasattr(value, '__len__'):
                    if len(value) > rules['max_length']:
                        errors.append(f"Field '{field}' exceeds maximum length")
                
                # パターンチェック
                if 'pattern' in rules and isinstance(value, str):
                    if not re.match(rules['pattern'], value):
                        errors.append(f"Field '{field}' format is invalid")
        
        return errors
    
    def _sanitize_data(self, data: Dict) -> Dict:
        """データのサニタイゼーション"""
        sanitized = {}
        
        for key, value in data.items():
            if isinstance(value, str):
                # HTMLエスケープ
                sanitized[key] = self._escape_html(value)
            else:
                sanitized[key] = value
        
        return sanitized
    
    def _escape_html(self, text: str) -> str:
        """HTMLエスケープ"""
        html_escape_table = {
            "&": "&amp;",
            '"': "&quot;",
            "'": "&#39;",
            ">": "&gt;",
            "<": "&lt;",
        }
        return "".join(html_escape_table.get(c, c) for c in text)
    
    def _detect_sql_injection(self, data: Dict) -> bool:
        """SQLインジェクション検出"""
        sql_patterns = [
            r"('|(\\')|(;)|(\\;))",  # クォートとセミコロン
            r"(union|select|insert|delete|update|drop|create|alter|exec|execute)",  # SQLキーワード
            r"(script|javascript|vbscript|onload|onerror|onclick)",  # スクリプト
            r"(<|>|\\|/\\*|\\*/)"  # その他の危険な文字
        ]
        
        for value in data.values():
            if isinstance(value, str):
                for pattern in sql_patterns:
                    if re.search(pattern, value.lower()):
                        return True
        
        return False
    
    def _log_security_event(self, event_type: str, details: Dict):
        """セキュリティイベントのログ記録"""
        event = {
            "timestamp": datetime.utcnow().isoformat(),
            "event_type": event_type,
            "details": details
        }
        self.audit_log.append(event)
        
        # 実際の実装では、より堅牢なログシステムを使用
        print(f"SECURITY EVENT: {event_type} - {details}")

# Flask アプリケーションでの使用例
app = Flask(__name__)

# セキュリティ設定
security_config = APISecurityConfig(
    rate_limit_requests=100,
    rate_limit_window=3600,
    jwt_secret="your-jwt-secret",
    cors_origins=["https://trusted-domain.com"],
    require_https=True
)

security = APISecurityFramework(security_config)

# ユーザー登録API（セキュア実装）
@app.route('/api/register', methods=['POST'])
@security.require_https
@security.cors_protection
@security.rate_limiting()
@security.input_validation({
    'email': {'required': True, 'type': str, 'pattern': r'^[\w\.-]+@[\w\.-]+\.\w+$'},
    'password': {'required': True, 'type': str, 'max_length': 128},
    'username': {'required': True, 'type': str, 'max_length': 50}
})
@security.sql_injection_protection
def register():
    data = g.validated_data
    
    # ユーザー登録ロジック
    return jsonify({
        "message": "User registered successfully",
        "user_id": "12345"
    }), 201

# 保護されたAPI エンドポイント
@app.route('/api/protected', methods=['GET'])
@security.require_https
@security.require_jwt
@security.rate_limiting()
def protected_endpoint():
    user_info = g.current_user
    
    return jsonify({
        "message": "Access granted",
        "user": user_info
    })

# API キーが必要なエンドポイント
@app.route('/api/admin', methods=['GET'])
@security.require_https
@security.require_api_key
@security.rate_limiting(lambda: g.api_key_info['user_id'])
def admin_endpoint():
    api_info = g.api_key_info
    
    return jsonify({
        "message": "Admin access granted",
        "api_key_owner": api_info['owner']
    })
```

### データベースセキュリティの実装

**セキュアなデータベース設計と実装**

```python
"""
データベースセキュリティの包括的実装
"""

import sqlite3
import psycopg2
from sqlalchemy import create_engine, text
from sqlalchemy.orm import sessionmaker
from cryptography.fernet import Fernet
import hashlib
import secrets
from typing import Dict, List, Optional, Any
import logging

class DatabaseSecurityFramework:
    """データベースセキュリティフレームワーク"""
    
    def __init__(self, db_config: Dict[str, str]):
        self.db_config = db_config
        self.encryption_key = self._generate_encryption_key()
        self.cipher_suite = Fernet(self.encryption_key)
        self.logger = logging.getLogger("DatabaseSecurity")
    
    def secure_connection(self) -> Any:
        """セキュアなデータベース接続"""
        
        # SSL/TLS暗号化の強制
        ssl_config = {
            'sslmode': 'require',
            'sslcert': self.db_config.get('ssl_cert'),
            'sslkey': self.db_config.get('ssl_key'),
            'sslrootcert': self.db_config.get('ssl_ca')
        }
        
        # 接続文字列の構築
        connection_string = (
            f"postgresql://{self.db_config['username']}:{self.db_config['password']}"
            f"@{self.db_config['host']}:{self.db_config['port']}/{self.db_config['database']}"
        )
        
        # SQLAlchemy エンジンの作成
        engine = create_engine(
            connection_string,
            connect_args=ssl_config,
            pool_pre_ping=True,  # 接続の健全性チェック
            pool_recycle=3600,   # 接続の再利用時間制限
            echo=False  # 本番環境では無効化
        )
        
        return engine
    
    def execute_secure_query(self, query: str, params: Dict[str, Any]) -> List[Dict]:
        """セキュアなクエリ実行"""
        
        engine = self.secure_connection()
        
        try:
            with engine.connect() as connection:
                # パラメータ化クエリの実行
                result = connection.execute(text(query), params)
                
                # 結果の取得
                columns = result.keys()
                rows = result.fetchall()
                
                # 辞書形式での結果返却
                return [dict(zip(columns, row)) for row in rows]
                
        except Exception as e:
            self.logger.error(f"Database query failed: {e}")
            raise
        finally:
            engine.dispose()
    
    def encrypt_sensitive_field(self, data: str) -> str:
        """機密フィールドの暗号化"""
        if not data:
            return data
        
        encrypted_data = self.cipher_suite.encrypt(data.encode())
        return encrypted_data.decode()
    
    def decrypt_sensitive_field(self, encrypted_data: str) -> str:
        """機密フィールドの復号化"""
        if not encrypted_data:
            return encrypted_data
        
        try:
            decrypted_data = self.cipher_suite.decrypt(encrypted_data.encode())
            return decrypted_data.decode()
        except Exception as e:
            self.logger.error(f"Decryption failed: {e}")
            return ""
    
    def hash_password(self, password: str) -> str:
        """パスワードのハッシュ化"""
        import bcrypt
        
        salt = bcrypt.gensalt(rounds=12)
        hashed = bcrypt.hashpw(password.encode('utf-8'), salt)
        return hashed.decode('utf-8')
    
    def verify_password(self, password: str, hashed: str) -> bool:
        """パスワードの検証"""
        import bcrypt
        
        try:
            return bcrypt.checkpw(password.encode('utf-8'), hashed.encode('utf-8'))
        except Exception as e:
            self.logger.error(f"Password verification failed: {e}")
            return False
    
    def audit_database_access(self, user_id: str, action: str, table_name: str, 
                             record_id: Optional[str] = None):
        """データベースアクセスの監査"""
        
        audit_query = """
            INSERT INTO audit_log (user_id, action, table_name, record_id, timestamp, ip_address)
            VALUES (:user_id, :action, :table_name, :record_id, :timestamp, :ip_address)
        """
        
        params = {
            'user_id': user_id,
            'action': action,
            'table_name': table_name,
            'record_id': record_id,
            'timestamp': datetime.utcnow(),
            'ip_address': self._get_client_ip()
        }
        
        try:
            self.execute_secure_query(audit_query, params)
            self.logger.info(f"Audit logged: {action} on {table_name} by {user_id}")
        except Exception as e:
            self.logger.error(f"Audit logging failed: {e}")
    
    def implement_row_level_security(self, user_id: str, user_role: str) -> Dict[str, str]:
        """行レベルセキュリティの実装"""
        
        # ユーザーの権限に基づいたWHERE句の生成
        rls_policies = {
            'admin': '',  # 全てのデータにアクセス可能
            'manager': 'WHERE department_id IN (SELECT department_id FROM user_departments WHERE user_id = :user_id)',
            'user': 'WHERE user_id = :user_id OR public_access = TRUE'
        }
        
        return {
            'where_clause': rls_policies.get(user_role, 'WHERE 1=0'),  # デフォルトはアクセス拒否
            'user_id': user_id
        }
    
    def validate_schema_changes(self, schema_change_sql: str) -> bool:
        """スキーマ変更の検証"""
        
        # 危険なSQL文のチェック
        dangerous_operations = [
            'DROP TABLE',
            'DROP DATABASE',
            'TRUNCATE',
            'DELETE FROM',
            'UPDATE',
            'ALTER TABLE'
        ]
        
        sql_upper = schema_change_sql.upper()
        
        for operation in dangerous_operations:
            if operation in sql_upper:
                self.logger.warning(f"Dangerous operation detected: {operation}")
                return False
        
        return True
    
    def backup_encryption(self, backup_file_path: str) -> str:
        """バックアップの暗号化"""
        
        # バックアップファイルの読み込み
        with open(backup_file_path, 'rb') as f:
            backup_data = f.read()
        
        # 暗号化
        encrypted_backup = self.cipher_suite.encrypt(backup_data)
        
        # 暗号化されたバックアップの保存
        encrypted_file_path = f"{backup_file_path}.encrypted"
        with open(encrypted_file_path, 'wb') as f:
            f.write(encrypted_backup)
        
        return encrypted_file_path
    
    def _generate_encryption_key(self) -> bytes:
        """暗号化キーの生成"""
        # 環境変数からキーを取得、なければ生成
        key = os.environ.get('DB_ENCRYPTION_KEY')
        if not key:
            key = Fernet.generate_key()
            # 実際の実装では、キーを安全に保存
            self.logger.warning("Generated new encryption key - store it securely!")
        
        return key if isinstance(key, bytes) else key.encode()
    
    def _get_client_ip(self) -> str:
        """クライアントIPアドレスの取得"""
        # フレームワークに応じた実装
        # Flask の場合: request.remote_addr
        return "127.0.0.1"  # プレースホルダー
```

### DevSecOpsパイプラインの実装

**セキュリティ統合CI/CDパイプライン**

```yaml
# .github/workflows/devsecops-pipeline.yml
name: DevSecOps Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  security-scan:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    # 1. 依存関係のセキュリティスキャン
    - name: Security Audit - Dependencies
      run: |
        # Python
        pip install safety
        safety check
        
        # Node.js
        npm audit
        
        # Go
        go list -json -m all | nancy sleuth
    
    # 2. シークレットスキャン
    - name: Secret Detection
      uses: trufflesecurity/trufflehog@main
      with:
        path: ./
        base: main
        head: HEAD
    
    # 3. 静的コード解析
    - name: Static Code Analysis
      run: |
        # Python - Bandit
        pip install bandit
        bandit -r . -f json -o bandit-report.json
        
        # JavaScript - ESLint Security
        npm install eslint-plugin-security
        eslint --ext .js,.jsx,.ts,.tsx --format json --output-file eslint-report.json .
        
        # Go - Gosec
        go install github.com/securecodewarrior/gosec/v2/cmd/gosec@latest
        gosec -fmt json -out gosec-report.json ./...
    
    # 4. Dockerイメージのセキュリティスキャン
    - name: Docker Security Scan
      run: |
        # Trivy でイメージスキャン
        docker build -t myapp:latest .
        docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
          aquasec/trivy image --format json --output trivy-report.json myapp:latest
    
    # 5. Infrastructure as Code スキャン
    - name: IaC Security Scan
      run: |
        # Terraform
        terraform init
        terraform plan -out=tfplan
        terraform show -json tfplan | jq '.' > tfplan.json
        
        # Checkov でスキャン
        pip install checkov
        checkov -f tfplan.json --framework terraform --output json --output-file checkov-report.json
    
    # 6. コンテナ設定のセキュリティチェック
    - name: Container Security Check
      run: |
        # Docker Bench Security
        docker run --rm --net host --pid host --userns host --cap-add audit_control \
          -v /var/lib:/var/lib:ro -v /var/run/docker.sock:/var/run/docker.sock:ro \
          -v /usr/lib/systemd:/usr/lib/systemd:ro -v /etc:/etc:ro \
          docker/docker-bench-security
    
    # 7. セキュリティレポートの統合
    - name: Security Report Integration
      run: |
        python3 << 'EOF'
        import json
        import os
        
        # セキュリティレポートの統合
        security_report = {
            "timestamp": "$(date -u +%Y-%m-%dT%H:%M:%SZ)",
            "commit": "${{ github.sha }}",
            "branch": "${{ github.ref }}",
            "scans": {}
        }
        
        # 各スキャン結果の読み込み
        scan_files = [
            ("bandit", "bandit-report.json"),
            ("eslint", "eslint-report.json"),
            ("gosec", "gosec-report.json"),
            ("trivy", "trivy-report.json"),
            ("checkov", "checkov-report.json")
        ]
        
        for scan_name, filename in scan_files:
            if os.path.exists(filename):
                with open(filename, 'r') as f:
                    security_report["scans"][scan_name] = json.load(f)
        
        # 統合レポートの保存
        with open("security-report.json", 'w') as f:
            json.dump(security_report, f, indent=2)
        
        # 重要度別の集計
        critical_issues = 0
        high_issues = 0
        medium_issues = 0
        low_issues = 0
        
        for scan_name, results in security_report["scans"].items():
            if scan_name == "bandit":
                for issue in results.get("results", []):
                    severity = issue.get("issue_severity", "").lower()
                    if severity == "high":
                        high_issues += 1
                    elif severity == "medium":
                        medium_issues += 1
                    elif severity == "low":
                        low_issues += 1
        
        print(f"Security Scan Summary:")
        print(f"Critical: {critical_issues}")
        print(f"High: {high_issues}")
        print(f"Medium: {medium_issues}")
        print(f"Low: {low_issues}")
        
        # 重要な脆弱性がある場合は失敗
        if critical_issues > 0 or high_issues > 0:
            print("❌ Critical or High severity issues found!")
            exit(1)
        else:
            print("✅ No critical or high severity issues found!")
        EOF
    
    # 8. セキュリティゲートの適用
    - name: Security Gate
      run: |
        # セキュリティ基準の確認
        python3 << 'EOF'
        import json
        import sys
        
        # セキュリティ基準の定義
        security_standards = {
            "max_critical_vulnerabilities": 0,
            "max_high_vulnerabilities": 0,
            "max_medium_vulnerabilities": 10,
            "required_security_headers": [
                "X-Frame-Options",
                "X-Content-Type-Options",
                "X-XSS-Protection",
                "Strict-Transport-Security",
                "Content-Security-Policy"
            ],
            "min_password_strength": 8,
            "require_encryption": True
        }
        
        # セキュリティレポートの読み込み
        with open("security-report.json", 'r') as f:
            report = json.load(f)
        
        # 基準チェック
        passed = True
        
        # 脆弱性数のチェック
        total_critical = 0
        total_high = 0
        total_medium = 0
        
        for scan_name, results in report["scans"].items():
            # 各スキャンツールの結果を正規化
            if scan_name == "bandit":
                for issue in results.get("results", []):
                    severity = issue.get("issue_severity", "").lower()
                    if severity == "high":
                        total_high += 1
                    elif severity == "medium":
                        total_medium += 1
        
        if total_critical > security_standards["max_critical_vulnerabilities"]:
            print(f"❌ Critical vulnerabilities: {total_critical} (max: {security_standards['max_critical_vulnerabilities']})")
            passed = False
        
        if total_high > security_standards["max_high_vulnerabilities"]:
            print(f"❌ High vulnerabilities: {total_high} (max: {security_standards['max_high_vulnerabilities']})")
            passed = False
        
        if total_medium > security_standards["max_medium_vulnerabilities"]:
            print(f"❌ Medium vulnerabilities: {total_medium} (max: {security_standards['max_medium_vulnerabilities']})")
            passed = False
        
        if passed:
            print("✅ Security gate passed!")
        else:
            print("❌ Security gate failed!")
            sys.exit(1)
        EOF
    
    # 9. セキュリティレポートのアップロード
    - name: Upload Security Reports
      uses: actions/upload-artifact@v3
      with:
        name: security-reports
        path: |
          security-report.json
          bandit-report.json
          eslint-report.json
          gosec-report.json
          trivy-report.json
          checkov-report.json
```

## 🔍 深掘り：プロの視点

### エンタープライズセキュリティアーキテクチャ

**Zero Trust アーキテクチャの実装**

```python
"""
Zero Trust セキュリティアーキテクチャの実装
"""

class ZeroTrustSecurityFramework:
    """Zero Trust セキュリティフレームワーク"""
    
    def __init__(self):
        self.trust_scores = {}
        self.device_registry = {}
        self.access_policies = {}
        self.continuous_monitoring = True
    
    def evaluate_trust_score(self, context: Dict[str, Any]) -> float:
        """信頼スコアの評価"""
        
        factors = {
            'device_compliance': self._evaluate_device_compliance(context),
            'user_behavior': self._evaluate_user_behavior(context),
            'network_location': self._evaluate_network_location(context),
            'access_pattern': self._evaluate_access_pattern(context),
            'threat_intelligence': self._evaluate_threat_intelligence(context)
        }
        
        # 重み付き平均による信頼スコア計算
        weights = {
            'device_compliance': 0.3,
            'user_behavior': 0.25,
            'network_location': 0.2,
            'access_pattern': 0.15,
            'threat_intelligence': 0.1
        }
        
        trust_score = sum(factors[f] * weights[f] for f in factors)
        return max(0.0, min(1.0, trust_score))
    
    def _evaluate_device_compliance(self, context: Dict[str, Any]) -> float:
        """デバイスコンプライアンスの評価"""
        device_id = context.get('device_id')
        if not device_id:
            return 0.0
        
        device_info = self.device_registry.get(device_id, {})
        
        compliance_checks = {
            'os_updated': device_info.get('os_updated', False),
            'antivirus_enabled': device_info.get('antivirus_enabled', False),
            'encryption_enabled': device_info.get('encryption_enabled', False),
            'managed_device': device_info.get('managed_device', False),
            'no_jailbreak': not device_info.get('jailbroken', True)
        }
        
        return sum(compliance_checks.values()) / len(compliance_checks)
    
    def _evaluate_user_behavior(self, context: Dict[str, Any]) -> float:
        """ユーザー行動の評価"""
        user_id = context.get('user_id')
        if not user_id:
            return 0.0
        
        # 行動パターンの分析
        behavior_score = 0.8  # ベースライン
        
        # 異常な時間帯のアクセス
        if self._is_unusual_time(context.get('timestamp')):
            behavior_score -= 0.2
        
        # 異常な地理的位置
        if self._is_unusual_location(user_id, context.get('location')):
            behavior_score -= 0.3
        
        # 異常なアクセスパターン
        if self._is_unusual_access_pattern(user_id, context.get('access_pattern')):
            behavior_score -= 0.2
        
        return max(0.0, behavior_score)
    
    def make_access_decision(self, context: Dict[str, Any]) -> Dict[str, Any]:
        """アクセス決定の実行"""
        
        trust_score = self.evaluate_trust_score(context)
        resource = context.get('resource')
        action = context.get('action')
        
        # リソースとアクションに基づく最小信頼スコア
        required_trust = self._get_required_trust_level(resource, action)
        
        decision = {
            'allowed': trust_score >= required_trust,
            'trust_score': trust_score,
            'required_trust': required_trust,
            'conditions': []
        }
        
        # 条件付きアクセス
        if trust_score >= required_trust * 0.8:  # 80%以上の場合
            if trust_score < required_trust:
                decision['conditions'] = [
                    'multi_factor_authentication_required',
                    'session_timeout_reduced',
                    'activity_monitoring_increased'
                ]
                decision['allowed'] = True
        
        return decision
```

## 📋 まとめとチェックポイント

### セキュアコーディングの重要ポイント

1. **セキュリティバイデザイン**
   - 設計段階からセキュリティを組み込む
   - 脅威モデリングの実施
   - 最小権限の原則の適用

2. **入力検証とサニタイゼーション**
   - 全ての入力を検証
   - ホワイトリスト方式の採用
   - 出力時のエスケープ処理

3. **認証と認可**
   - 強力な認証機構の実装
   - 多要素認証の導入
   - 細かな権限制御

4. **セキュアな通信**
   - TLS/SSL暗号化の必須化
   - 証明書の適切な検証
   - セキュアなプロトコルの使用

### セルフチェック項目

**基礎レベル（90%の理解が必要）**
- [ ] セキュアコーディングの基本原則を理解している
- [ ] 一般的な脆弱性（OWASP Top 10）を説明できる
- [ ] 入力検証の重要性と実装方法を知っている
- [ ] パスワードのセキュアな処理方法を理解している

**中級レベル（80%の理解が必要）**
- [ ] 言語固有のセキュリティパターンを実装できる
- [ ] APIセキュリティの実装ができる
- [ ] データベースセキュリティを適切に実装できる
- [ ] セキュリティテストの自動化ができる

**上級レベル（70%の理解が必要）**
- [ ] セキュリティバイデザインの原則を実践できる
- [ ] Zero Trustアーキテクチャを理解している
- [ ] DevSecOpsパイプラインを構築できる
- [ ] セキュリティ監査とコンプライアンスに対応できる

**マスターレベル（60%の理解が必要）**
- [ ] エンタープライズセキュリティアーキテクチャを設計できる
- [ ] セキュリティインシデントに対応できる
- [ ] セキュリティ文化の構築と推進ができる
- [ ] 最新のセキュリティ脅威に対応できる

## 🔗 関連知識・発展学習

### 必読書籍・資料

**セキュリティ基礎**
- 「The Web Application Hacker's Handbook」
- 「Secure Coding: Principles and Practices」
- 「The Tangled Web: A Guide to Securing Modern Web Applications」

**実践的なセキュリティ**
- 「Security Engineering: A Guide to Building Dependable Distributed Systems」
- 「Threat Modeling: Designing for Security」
- 「Building Secure and Reliable Systems」

### 技術標準・フレームワーク

**セキュリティ標準**
- OWASP Application Security Verification Standard (ASVS)
- NIST Cybersecurity Framework
- ISO/IEC 27001/27002

**実装フレームワーク**
- OWASP Top 10 対策
- SANS Top 25 Software Errors
- CWE/SANS Top 25 Most Dangerous Programming Errors

### 認定資格

**セキュリティ認定**
- CISSP (Certified Information Systems Security Professional)
- CEH (Certified Ethical Hacker)
- GWEB (GIAC Web Application Penetration Tester)
- OSCP (Offensive Security Certified Professional)

### 継続的な学習リソース

**技術コミュニティ**
- OWASP Local Chapters
- Security BSides Events
- DEF CON Conference
- Black Hat Conference

**実践プラットフォーム**
- HackerOne（Bug Bounty）
- Bugcrowd（Bug Bounty）
- TryHackMe（Hands-on Learning）
- OverTheWire（Wargames）

**オンライン教育**
- SANS Training
- Pluralsight Security Path
- Coursera Cybersecurity Specialization
- edX MIT Introduction to Computer Science and Programming

セキュアコーディングは、現代のソフトウェア開発において不可欠なスキルです。攻撃者が常に新しい手法を開発する中、防御側も継続的に学習し、最新のセキュリティ技術を習得する必要があります。この章で学んだ知識を基に、実際のプロジェクトでセキュアな実装を実践し、セキュリティ文化の構築に貢献してください。
