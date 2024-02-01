#package com.example.myrestaurant

import android.Manifest
import android.bluetooth.BluetoothAdapter
import android.content.Context
import android.content.Intent
import android.content.pm.PackageManager
import android.net.wifi.p2p.WifiP2pManager
import android.os.Bundle
import android.widget.Button
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.ActivityCompat
import androidx.core.content.ContextCompat
import com.google.zxing.integration.android.IntentIntegrator

class MainActivity : AppCompatActivity() {

    private val WIFI_PERMISSION_REQUEST_CODE = 101
    private val BLUETOOTH_PERMISSION_REQUEST_CODE = 102

    private lateinit var wifiP2pManager: WifiP2pManager
    private lateinit var bluetoothAdapter: BluetoothAdapter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val scanButton: Button = findViewById(R.id.scanButton)
        val orderButton: Button = findViewById(R.id.orderButton)
        val callWaiterButton: Button = findViewById(R.id.callWaiterButton)

        wifiP2pManager = getSystemService(Context.WIFI_P2P_SERVICE) as WifiP2pManager
        bluetoothAdapter = BluetoothAdapter.getDefaultAdapter()

        requestPermissions()

        scanButton.setOnClickListener { startQRScan() }
        // Add onClickListeners for orderButton and callWaiterButton
    }

    private fun startQRScan() {
        val integrator = IntentIntegrator(this)
        integrator.setPrompt("Scan a QR code")
        integrator.setOrientationLocked(false)
        integrator.initiateScan()
    }

    private fun requestPermissions() {
        if (!hasWiFiPermissions()) {
            requestWiFiPermissions()
        } else {
            connectToWiFi()
        }

        if (!hasBluetoothPermissions()) {
            requestBluetoothPermissions()
        } else {
            connectToBluetooth()
        }
    }

    private fun hasWiFiPermissions(): Boolean {
        return ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_WIFI_STATE) ==
                PackageManager.PERMISSION_GRANTED &&
                ContextCompat.checkSelfPermission(this, Manifest.permission.CHANGE_WIFI_STATE) ==
                PackageManager.PERMISSION_GRANTED
    }

    private fun requestWiFiPermissions() {
        ActivityCompat.requestPermissions(this, arrayOf(
            Manifest.permission.ACCESS_WIFI_STATE,
            Manifest.permission.CHANGE_WIFI_STATE
        ), WIFI_PERMISSION_REQUEST_CODE)
    }

    private fun connectToWiFi() {
        // Implement WiFi Direct connection logic here
    }

    private fun hasBluetoothPermissions(): Boolean {
        return ContextCompat.checkSelfPermission(this, Manifest.permission.BLUETOOTH) ==
                PackageManager.PERMISSION_GRANTED &&
                ContextCompat.checkSelfPermission(this, Manifest.permission.BLUETOOTH_ADMIN) ==
                PackageManager.PERMISSION_GRANTED
    }

    private fun requestBluetoothPermissions() {
        ActivityCompat.requestPermissions(this, arrayOf(
            Manifest.permission.BLUETOOTH,
            Manifest.permission.BLUETOOTH_ADMIN
        ), BLUETOOTH_PERMISSION_REQUEST_CODE)
    }

    private fun connectToBluetooth() {
        // Implement Bluetooth connection logic here
    }

    override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)
        when (requestCode) {
            WIFI_PERMISSION_REQUEST_CODE -> {
                if (grantResults.all { it == PackageManager.PERMISSION_GRANTED }) {
                    connectToWiFi()
                } else {
                    Toast.makeText(this, "WiFi permissions are required", Toast.LENGTH_SHORT).show()
                }
            }
            BLUETOOTH_PERMISSION_REQUEST_CODE -> {
                if (grantResults.all { it == PackageManager.PERMISSION_GRANTED }) {
                    connectToBluetooth()
                } else {
                    Toast.makeText(this, "Bluetooth permissions are required", Toast.LENGTH_SHORT).show()
                }
            }
        }
    }
} Convenience-
Smart restaurant menu and waiter signaling 
