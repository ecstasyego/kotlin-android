## Examples
### Example01: *.kt
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.widget.Button
import android.widget.LinearLayout
import android.widget.Toast
import androidx.activity.ComponentActivity
import com.android.billingclient.api.*

class MainActivity : ComponentActivity() {
    private lateinit var billingClient: BillingClient

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val layout = LinearLayout(this)
        val button = Button(this).apply {
            text = "Premium Subscription"
            setOnClickListener {
                launchPurchaseFlow()
            }
        }
        layout.addView(button)
        setContentView(layout)

        // BillingClient 초기화 및 연결
        billingClient = BillingClient.newBuilder(this)
            .enablePendingPurchases()
            .setListener { billingResult, purchases ->
                // 결제 결과 콜백 처리 (여기선 생략)
            }
            .build()

        billingClient.startConnection(object : BillingClientStateListener {
            override fun onBillingSetupFinished(billingResult: BillingResult) {
                if (billingResult.responseCode == BillingClient.BillingResponseCode.OK) {
                    Toast.makeText(this@MainActivity, "Billing 연결 완료", Toast.LENGTH_SHORT).show()
                } else {
                    Toast.makeText(this@MainActivity, "Billing 연결 실패", Toast.LENGTH_SHORT).show()
                }
            }

            override fun onBillingServiceDisconnected() {
                Toast.makeText(this@MainActivity, "Billing 서비스 연결 끊김", Toast.LENGTH_SHORT).show()
            }
        })
    }

    private fun launchPurchaseFlow() {
        if (!billingClient.isReady) {
            Toast.makeText(this, "BillingClient가 준비되지 않았습니다.", Toast.LENGTH_SHORT).show()
            return
        }

        val productList = listOf(
            QueryProductDetailsParams.Product.newBuilder()
                .setProductId("premium_monthly") // Google Play Console에 등록된 ID
                .setProductType(BillingClient.ProductType.SUBS)
                .build()
        )

        val params = QueryProductDetailsParams.newBuilder()
            .setProductList(productList)
            .build()

        billingClient.queryProductDetailsAsync(params) { billingResult, productDetailsList ->
            if (billingResult.responseCode == BillingClient.BillingResponseCode.OK && productDetailsList.isNotEmpty()) {
                val productDetails = productDetailsList[0]

                val billingFlowParams = BillingFlowParams.newBuilder()
                    .setProductDetailsParamsList(
                        listOf(
                            BillingFlowParams.ProductDetailsParams.newBuilder()
                                .setProductDetails(productDetails)
                                .build()
                        )
                    )
                    .build()

                billingClient.launchBillingFlow(this, billingFlowParams)
            } else {
                Toast.makeText(this, "상품 정보를 불러올 수 없습니다.", Toast.LENGTH_SHORT).show()
            }
        }
    }
}
```

`build.gradle.kts`
```kotlin
dependencies {
    implementation("com.android.billingclient:billing-ktx:6.0.1")
}
```
