# no-control-regex

作成日 2025/10/14、更新日 2025/10/15

## 1. 公式サイト（英語）を読む

[no-control-regex - ESLint](https://eslint.org/docs/latest/rules/no-control-regex)

正規表現の中で、制御文字を許さないルール

設定ファイルの中で、`@eslint/js`の`recommended`設定を使うと、このルールが有効になる

制御文字は、特別かつ目に見えず、ASCIIレンジで0から31までの文字である

制御文字はがJavaScriptで使われることは稀であり、したがって、正規表現の要素にこれらの文字が含まれていることはミスである可能性がある

正規表現のパターンの中で、以下の使用は、エラーの可能性とみなされ、それゆえ、このルールでは禁止される

- Hexadecimal character escapes from \x00 to \x1F.
- Unicode character escapes from \u0000 to \u001F.
- Unicode code point escapes from \u{0} to \u{1F}.
- Unescaped raw characters from U+0000 to U+001F.

## 2. 制御文字の一覧

```text
\u0000: NULL (NUL)
\u0001: Start of Heading (SOH)
\u0002: Start of Text (STX)
\u0003: End of Text (ETX)
\u0004: End of Transmission (EOT)
\u0005: Enquiry (ENQ)
\u0006: Acknowledge (ACK)
\u0007: Bell (BEL)
\u0008: Backspace (BS)
\u0009: Horizontal Tab (HT)
\u000A: Line Feed (LF)
\u000B: Vertical Tab (VT)
\u000C: Form Feed (FF)
\u000D: Carriage Return (CR)
\u000E: Shift Out (SO)
\u000F: Shift In (SI)
\u0010: Data Link Escape (DLE)
\u0011: Device Control 1 (DC1)
\u0012: Device Control 2 (DC2)
\u0013: Device Control 3 (DC3)
\u0014: Device Control 4 (DC4)
\u0015: Negative Acknowledge (NAK)
\u0016: Synchronous Idle (SYN)
\u0017: End of Transmission Block (ETB)
\u0018: Cancel (CAN)
\u0019: End of Medium (EM)
\u001A: Substitute (SUB)
\u001B: Escape (ESC)
\u001C: File Separator (FS)
\u001D: Group Separator (GS)
\u001E: Record Separator (RS)
\u001F: Unit Separator (US)
```
