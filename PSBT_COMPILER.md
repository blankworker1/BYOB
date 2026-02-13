# PSBT Compiler App - Developer Blueprint
**Minimalist Android App for SeedSigner Integration**

---

## 🎯 Overview

A watch-only Bitcoin wallet app that creates PSBTs (Partially Signed Bitcoin Transactions) for air-gapped signing with SeedSigner hardware. The app operates entirely in memory with no persistent storage, connecting to a community Bitcoin node via Tailscale for blockchain data.

**Core Philosophy:** Ephemeral, stateless, minimal attack surface.

---

## 🌟 User Experience (UX) - 3 Screens

### **Screen 1: Launch - Scan Membership Card**

**Purpose:** Ephemeral wallet initialization

**UI Elements:**
- Centered logo/app name
- Primary action: "Scan Membership Card" button
- Camera opens on tap

**Flow:**
1. User taps button → camera activates
2. Scans xpub/tpub QR code from membership card
3. App validates QR format and network compatibility
4. Connects to community node via Tailscale
5. Builds descriptor and fetches balance
6. Transitions to Wallet Home

**Validations:**
- ✅ Valid xpub (mainnet) or tpub (testnet) format
- ✅ Network matches app configuration (testnet `tb1...` vs mainnet `bc1...`)
- ✅ Community node is reachable via Tailscale

**Error Handling:**
- "Invalid QR code - not a valid xpub/tpub"
- "Network mismatch - this is a mainnet xpub, app is in testnet mode"
- "Cannot reach community node - ensure Tailscale is connected"

---

### **Screen 2: Wallet Home**

**Purpose:** View-only wallet dashboard

**UI Elements:**
- **Balance display** (large, centered): "0.05000000 BTC"
- **Connection status indicator** (top-right): Green dot ● = connected, Red dot ● = disconnected
- **Network badge** (top-left): "Testnet" or "Mainnet"
- **Transaction history** (scrollable list, optional):
  - Recent transactions with timestamps, amounts, confirmations
  - Tap to view TXID
- **Details toggle** (collapsible section):
  - "Show UTXOs ↓" → expands to list individual unspent outputs
  - Hidden by default for simplicity
- **Primary button**: "Send" (bottom, full-width)
- **Secondary button**: "Clear Session" (text link, top-right corner)

**Behavior:**
- Read-only view - no key material stored
- Balance auto-refreshes on return from Send screen
- "Clear Session" → wipes all data, returns to Launch screen
- App backgrounding (onPause) → auto-wipes session for security

**Advanced UTXO View (collapsed by default):**
```
UTXOs (3):
├─ 0.02 BTC - tx:abc123...def - Output #0 - 6 confirmations
├─ 0.02 BTC - tx:456789...ghi - Output #1 - 12 confirmations
└─ 0.01 BTC - tx:jkl012...mno - Output #0 - 3 confirmations
```

---

### **Screen 3: Send Transaction (All-in-One Flow)**

**Purpose:** Build PSBT, sign with SeedSigner, broadcast to network

**UI - Step-by-step vertical flow:**

#### **Step 1: Scan Recipient Address**
- Button: "Scan Recipient QR Code"
- Camera opens → scans Bitcoin address QR
- Displays scanned address for confirmation
- Validation: Must match network (bc1/tb1 prefix)

#### **Step 2: Enter Amount & Fee**
- **Amount input field**: "Amount to send (BTC)"
- **Fee rate selector**: 
  - Fetched from node: "Fast (10 sat/vB) | Medium (5 sat/vB) | Slow (2 sat/vB)"
  - Or manual entry: "Custom fee rate (sat/vB)"
- **Calculated fee display**: "Network fee: 0.00001234 BTC"
- **Total display**: "Total: 0.01001234 BTC (Amount + Fee)"

#### **Step 3: Review Transaction**
- Confirmation summary:
  ```
  Sending: 0.01000000 BTC
  To: tb1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh
  Fee: 0.00001234 BTC
  Total from wallet: 0.01001234 BTC
  ```
- Button: "Build PSBT"

#### **Step 4: Display PSBT QR for SeedSigner**
- **Animated QR code** (multi-part if needed for large PSBTs)
- Label: "Scan this QR with your SeedSigner to sign"
- Instruction text: "After signing, SeedSigner will display a signed PSBT QR"
- Button: "I've Signed - Scan Signed PSBT"

#### **Step 5: Scan Signed PSBT**
- Camera opens → scans signed PSBT QR from SeedSigner
- Validates PSBT structure and signatures
- Verifies inputs match original unsigned PSBT

#### **Step 6: Broadcast & Confirmation**
- Broadcasts transaction to community node
- Success screen:
  ```
  ✅ Transaction Broadcast
  TXID: a1b2c3d4e5f6...
  Status: 0 confirmations (pending)
  
  [View on Explorer] [Return to Wallet]
  ```
- Auto-returns to Wallet Home after 5 seconds

**Error Handling:**
- "Invalid recipient address for this network"
- "Insufficient balance - need 0.015 BTC, only have 0.01 BTC"
- "PSBT validation failed - inputs don't match"
- "Broadcast failed - node rejected transaction"

---

## 🏗️ Technical Architecture

### **System Components (4 Core Modules)**

#### **1. WalletSession**
Manages ephemeral wallet state in memory.

**Responsibilities:**
- Stores xpub/tpub descriptor
- Generates receive addresses using BIP84 (Native SegWit)
- Tracks current session balance
- Maintains transaction history for current session
- Provides session cleanup method

**Key Methods:**
```kotlin
class WalletSession {
    fun initializeFromXpub(xpub: String, network: Network)
    fun getBalance(): Long // satoshis
    fun getAddresses(count: Int): List<String>
    fun getUTXOs(): List<UTXO>
    fun getTransactionHistory(): List<Transaction>
    fun clearSession()
}
```

**Data Structures:**
```kotlin
data class UTXO(
    val txid: String,
    val vout: Int,
    val amount: Long, // satoshis
    val address: String,
    val confirmations: Int
)

data class Transaction(
    val txid: String,
    val timestamp: Long,
    val amount: Long,
    val confirmations: Int,
    val type: TransactionType // RECEIVED, SENT
)
```

---

#### **2. ElectrumClient**
Handles all communication with community Bitcoin node via Electrum protocol over Tailscale.

**Responsibilities:**
- Establishes connection to node (SSL/TCP)
- Monitors connection health (includes health monitoring)
- Fetches balance and UTXOs for addresses
- Fetches fee estimates
- Broadcasts signed transactions
- Retrieves transaction history

**Connection:**
- Protocol: Electrum JSON-RPC over TCP/SSL
- Host: Community node Tailscale IP (e.g., `100.x.x.x:50002`)
- Timeout: 10 seconds for initial connection
- Retry: 3 attempts with exponential backoff

**Key Methods:**
```kotlin
class ElectrumClient(private val host: String, private val port: Int) {
    fun connect(): Boolean
    fun isConnected(): Boolean
    fun getBalance(addresses: List<String>): Long
    fun getUTXOs(addresses: List<String>): List<UTXO>
    fun getFeeEstimates(): FeeEstimates
    fun broadcastTransaction(signedTx: String): String // returns TXID
    fun getTransactionHistory(addresses: List<String>): List<Transaction>
    fun disconnect()
}

data class FeeEstimates(
    val fast: Int,    // sat/vB for ~1 block
    val medium: Int,  // sat/vB for ~3 blocks
    val slow: Int     // sat/vB for ~6 blocks
)
```

---

#### **3. PSBTBuilder**
Constructs unsigned PSBTs in memory for SeedSigner signing.

**Responsibilities:**
- Selects UTXOs for transaction inputs (coin selection)
- Calculates change amount
- Builds PSBT structure
- Validates PSBT format
- Encodes/decodes PSBTs to/from base64

**Coin Selection Strategy:**
- **Simple approach (MVP):** Largest-first until amount + fee is covered
- **Future enhancement:** Branch and bound for privacy

**Key Methods:**
```kotlin
class PSBTBuilder(private val session: WalletSession) {
    fun buildPSBT(
        recipient: String,
        amount: Long,
        feeRate: Int
    ): String // returns base64-encoded PSBT
    
    fun validateSignedPSBT(
        unsignedPSBT: String,
        signedPSBT: String
    ): Boolean
    
    fun extractTransaction(signedPSBT: String): String // hex-encoded tx
}
```

**PSBT Structure:**
- Inputs: Selected UTXOs with scriptPubKey, witness UTXO data
- Outputs: Recipient + change address (if needed)
- Fee: Calculated from (inputs - outputs)
- BIP 174 compliance

---

#### **4. QRCodec**
Handles QR code encoding and decoding for all app interactions.

**Responsibilities:**
- Decode xpub/tpub from membership card
- Decode recipient Bitcoin addresses
- Encode unsigned PSBTs to animated QR (if multi-part)
- Decode signed PSBTs from SeedSigner
- Handle UR (Uniform Resources) format for large PSBTs

**QR Standards:**
- xpub/tpub: Plain text QR
- Addresses: Plain text or BIP21 URI
- PSBTs: Base64 or UR format (BC-UR for multi-part)

**Key Methods:**
```kotlin
class QRCodec {
    fun decodeXpub(qrData: String): String
    fun decodeAddress(qrData: String): String
    fun encodePSBT(psbt: String): List<Bitmap> // returns animated QR frames
    fun decodePSBT(qrData: String): String
}
```

**Animated QR Handling:**
- Single-part: Static QR if PSBT < 2KB
- Multi-part: Animated QR sequence using BC-UR fountain codes
- Frame rate: 5 FPS for SeedSigner compatibility

---

## 🔐 Security Requirements

### **Critical Security Features:**

1. **No Persistent Storage**
   - All data stored only in RAM
   - No files written to disk
   - No database, no shared preferences for wallet data
   - Session auto-clears on app backgrounding

2. **Network Validation**
   ```kotlin
   fun validateNetwork(xpub: String, expectedNetwork: Network): Boolean {
       val prefix = xpub.substring(0, 4)
       return when (expectedNetwork) {
           Network.MAINNET -> prefix == "xpub" || prefix == "zpub"
           Network.TESTNET -> prefix == "tpub" || prefix == "vpub"
       }
   }
   ```

3. **Address Validation**
   ```kotlin
   fun validateAddress(address: String, network: Network): Boolean {
       val prefix = address.substring(0, 3)
       return when (network) {
           Network.MAINNET -> prefix == "bc1"
           Network.TESTNET -> prefix == "tb1"
       }
   }
   ```

4. **PSBT Verification**
   - Before broadcast, verify:
     - Signed PSBT inputs match unsigned PSBT inputs (same TXIDs, vouts)
     - Output amounts haven't changed
     - Signatures are present for all inputs
     - Transaction is valid (sum of inputs ≥ sum of outputs + fee)

5. **Tailscale Connection Verification**
   ```kotlin
   fun verifyTailscaleConnection(): Boolean {
       // Check if connected to Tailscale network
       // Verify node IP is in Tailscale range (100.x.x.x)
       // Reject connections to public IPs
   }
   ```

6. **Amount Confirmation**
   - Always show human-readable confirmation before building PSBT
   - Display amounts in BTC (not satoshis) for user clarity
   - Highlight total amount leaving wallet (amount + fee)

---

## 📱 Android Implementation Details

### **Technology Stack:**
- **Language:** Kotlin
- **Min SDK:** API 26 (Android 8.0)
- **Target SDK:** API 34 (Android 14)
- **Architecture:** MVVM (Model-View-ViewModel)
- **DI:** Hilt or Koin
- **UI:** Jetpack Compose (recommended) or XML layouts with Material Design 3

### **Key Libraries:**

```gradle
dependencies {
    // Bitcoin libraries
    implementation "org.bitcoinj:bitcoinj-core:0.16.2"
    
    // QR code handling
    implementation "com.google.zxing:core:3.5.1"
    implementation "com.journeyapps:zxing-android-embedded:4.3.0"
    
    // Networking
    implementation "com.squareup.okhttp3:okhttp:4.11.0"
    
    // Coroutines for async operations
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3"
    
    // ViewModel and LiveData
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.2"
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:2.6.2"
}
```

### **App Lifecycle Management:**

```kotlin
class MainActivity : AppCompatActivity() {
    
    private lateinit var walletSession: WalletSession
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Initialize components
    }
    
    override fun onPause() {
        super.onPause()
        // CRITICAL: Clear session when app backgrounds
        walletSession.clearSession()
        // Return to launch screen
        navigateToLaunch()
    }
    
    override fun onDestroy() {
        super.onDestroy()
        // Disconnect from node
        electrumClient.disconnect()
    }
}
```

### **Permissions Required:**

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

---

## 🎨 UI/UX Guidelines

### **Design Principles:**
1. **Large touch targets** - Minimum 48dp for all interactive elements
2. **High contrast** - Ensure readability in bright outdoor conditions
3. **Haptic feedback** - Vibrate on successful QR scans
4. **Clear error states** - Red text with specific actionable messages
5. **Loading indicators** - Show progress for network operations

### **Color Scheme (Suggested):**
- Primary: Bitcoin Orange (#F7931A)
- Background: Dark theme (#121212) or Light (#FFFFFF)
- Success: Green (#4CAF50)
- Error: Red (#F44336)
- Connected: Green dot (#4CAF50)
- Disconnected: Red dot (#F44336)

### **Typography:**
- Balance amounts: 32sp, bold, monospace font
- Addresses: 14sp, monospace font
- Body text: 16sp, regular
- Error messages: 14sp, medium weight

### **Camera QR Scanner:**
- Fullscreen overlay with centered square viewfinder
- Crosshair or grid overlay for alignment
- Auto-focus and flash toggle
- Success animation on valid scan (green checkmark)

---

## 🧪 Testing Requirements

### **Unit Tests:**
- WalletSession: Descriptor generation, address derivation
- PSBTBuilder: Coin selection, fee calculation, PSBT structure
- QRCodec: Encoding/decoding edge cases
- Network validation functions

### **Integration Tests:**
- ElectrumClient: Connection, balance fetching, broadcasting (use testnet)
- End-to-end PSBT flow: Build → encode → decode → validate

### **Security Tests:**
- Verify session clears on app background
- Validate no data written to disk
- Test network mismatch rejection
- Test invalid PSBT rejection

### **Manual Testing Checklist:**
- [ ] Scan valid xpub → displays balance correctly
- [ ] Scan invalid QR → shows appropriate error
- [ ] Build PSBT → QR displays and is scannable by SeedSigner
- [ ] Broadcast signed PSBT → transaction appears in mempool
- [ ] Background app → session clears and returns to launch
- [ ] Disconnect Tailscale → shows connection error
- [ ] Testnet/mainnet toggle works correctly

---

## 🚀 Development Roadmap

### Phase 1: MVP
**Goal:** Basic send/receive functionality

- [ ] Screen 1: Launch + xpub scanning
- [ ] Screen 2: Wallet Home with balance display
- [ ] Screen 3: Send flow (simplified - single UTXO selection)
- [ ] ElectrumClient: Connect, fetch balance, broadcast
- [ ] PSBTBuilder: Basic PSBT construction
- [ ] QRCodec: Static QR encoding/decoding
- [ ] Manual session clearing

**Deliverable:** APK that can scan xpub, display balance, create and broadcast simple PSBTs

### Phase 2: Polish
**Goal:** Production-ready UX

- [ ] Auto-session clearing on app background
- [ ] Improved coin selection algorithm
- [ ] Animated QR support for large PSBTs
- [ ] Transaction history display
- [ ] Advanced UTXO view (collapsible)
- [ ] Fee estimation from node
- [ ] Connection health monitoring with auto-retry
- [ ] Error handling and user feedback improvements

**Deliverable:** User-friendly app ready for community beta testing

### Phase 3: Advanced Features
**Goal:** Power user functionality

- [ ] Testnet/mainnet toggle in settings

**Deliverable:** Feature-complete app with advanced options

---

## 📊 Data Flow Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                         User Journey                          │
└──────────────────────────────────────────────────────────────┘

[Membership Card QR]
        ↓ scan
┌─────────────────┐
│  Launch Screen  │
│   Camera opens  │
└────────┬────────┘
         ↓ validates xpub
         ↓ connects to node
┌─────────────────┐
│  WalletSession  │──────→ ElectrumClient ──→ [Community Node]
│  • Descriptor   │         ↓ fetch UTXOs         (Tailscale)
│  • Balance      │         ↓ get balance
│  • UTXOs        │←────────┘
└────────┬────────┘
         ↓
┌─────────────────┐
│  Wallet Home    │
│  Balance: 0.05  │  ← displays in-memory data
│  ● Connected    │
│  [Send]         │
└────────┬────────┘
         ↓ tap Send
┌─────────────────┐
│  Send Screen    │
│  Step 1: Scan   │──→ [Recipient Address QR]
│  Step 2: Amount │
│  Step 3: Review │
└────────┬────────┘
         ↓ tap Build PSBT
┌─────────────────┐
│  PSBTBuilder    │
│  • Select UTXOs │
│  • Calculate Fee│
│  • Build PSBT   │
└────────┬────────┘
         ↓ encode to QR
┌─────────────────┐
│  QRCodec        │
│  Animated QR    │──→ [SeedSigner Scans]
│  (unsigned PSBT)│         ↓ signs
└────────┬────────┘         ↓
         ↓ scan signed      ↓
┌─────────────────┐    [Signed PSBT QR]
│  Validate PSBT  │←───────┘
│  • Match inputs │
│  • Verify sigs  │
└────────┬────────┘
         ↓ valid
┌─────────────────┐
│ ElectrumClient  │
│ Broadcast TX    │──→ [Community Node] ──→ [Bitcoin Network]
└────────┬────────┘
         ↓ returns TXID
┌─────────────────┐
│  Success Screen │
│  TXID: abc123...│
│  0 confirmations│
└────────┬────────┘
         ↓ auto-return (5s)
┌─────────────────┐
│  Wallet Home    │
│  (updated)      │
└─────────────────┘

[App Backgrounds] → onPause() → clearSession() → [Launch Screen]
```

---

## 🔧 Configuration

### **Network Configuration:**

```kotlin
// config/NetworkConfig.kt
object NetworkConfig {
    enum class Network {
        MAINNET,
        TESTNET
    }
    
    // Set this at compile time or via build variant
    const val CURRENT_NETWORK = Network.TESTNET
    
    // Electrum server details
    const val ELECTRUM_HOST = "100.x.x.x" // Tailscale IP
    const val ELECTRUM_PORT_TESTNET = 60002
    const val ELECTRUM_PORT_MAINNET = 50002
    
    fun getElectrumPort(): Int {
        return when (CURRENT_NETWORK) {
            Network.TESTNET -> ELECTRUM_PORT_TESTNET
            Network.MAINNET -> ELECTRUM_PORT_MAINNET
        }
    }
}
```

### **Build Variants:**

```gradle
// app/build.gradle
android {
    buildTypes {
        debug {
            buildConfigField "String", "NETWORK", '"TESTNET"'
            buildConfigField "String", "ELECTRUM_HOST", '"100.x.x.x"'
        }
        release {
            buildConfigField "String", "NETWORK", '"MAINNET"'
            buildConfigField "String", "ELECTRUM_HOST", '"100.y.y.y"'
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt')
        }
    }
}
```

---

## ⚠️ Known Limitations & Future Enhancements

### **MVP Limitations:**
1. **Single-signature only** - No multisig support
2. **BIP84 only** - Native SegWit (bc1/tb1) addresses only
3. **Simple coin selection** - Not optimized for privacy
4. **No RBF** - Cannot bump fees after broadcast
5. **Single xpub per session** - Cannot manage multiple wallets simultaneously


---

## 📝 Developer Notes

### **Code Style:**
- Follow [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html)
- Use meaningful variable names (avoid single letters except loop counters)
- Comment complex Bitcoin logic (PSBT structure, descriptor derivation)
- Keep functions under 50 lines where possible

**Error Handling Philosophy:** - **User-facing errors:** Always provide actionable guidance - ❌ "Error 404" - ✅ "Cannot reach node - check Tailscale connection" - **Developer errors:** Log stack traces for debugging - **Network errors:** Implement retry logic with exponential backoff 

### **Performance Considerations:** - UTXO fetching: Batch requests, don't query per address - QR generation: Offload to background thread - Balance updates: Debounce rapid refreshes (max 1/second) 

### **Accessibility:** - All interactive elements have content descriptions - Support TalkBack screen reader - Ensure 4.5:1 contrast ratio for text --- 

## 📄 License & Legal **Recommended License:** MIT or GPL-3.0 (for Bitcoin ecosystem compatibility) 


## Summary Checklist 


**Architecture:** - [x] 3-screen design (Launch, Wallet Home, Send) - [x] 4 core components (WalletSession, ElectrumClient, PSBTBuilder, QRCodec) - [x] No backend server - fully frontend - [x] Ephemeral session management 

**Security:** - [x] No persistent storage - [x] Watch-only wallet (xpub only) - [x] Network validation (testnet/mainnet) - [x] PSBT input/output verification - [x] Auto-session clearing on background - [x] Tailscale-only connectivity 

**Functionality:** - [x] Scan xpub → display balance - [x] Build unsigned PSBT → QR output - [x] Scan signed PSBT → broadcast - [x] Connection health monitoring - [x] Fee estimation from node **UX:** - [x] Simple linear flow - [x] Clear error messages - [x] Large touch targets - [x] Animated QR support for large PSBTs - [x] Haptic feedback on scans --- 


## 🎯 Next Steps for Developer 

1. **Set up development environment:** - Install Android Studio - Configure Kotlin plugin - Set up Tailscale on test device 

2. **Create project structure:** ``` app/ ├── ui/ │ ├── launch/ │ ├── home/ │ └── send/ ├── core/ │ ├── WalletSession.kt │ ├── ElectrumClient.kt │ ├── PSBTBuilder.kt │ └── QRCodec.kt ├── config/ │ └── NetworkConfig.kt └── utils/ ├── Validators.kt └── Extensions.kt









