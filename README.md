# PS4-Controller-Raspberry-Pi-Bluetooth

# Connect a PS4 Controller to Raspberry Pi via Bluetooth (with Auto-Reconnect)

This guide explains how to connect a **PS4 DualShock controller** to a **Raspberry Pi** via **Bluetooth** using the **Blueman GUI**, and how to make it **auto-reconnect on boot** — so you never have to re-pair it again.

---

## Requirements

- Raspberry Pi with Bluetooth (internal or USB adapter)
- PS4 controller (DualShock)
- Ubuntu
- Blueman installed

---

## Step 1: Install Blueman (Bluetooth Manager)

```bash
sudo apt update
sudo apt install blueman
```

Launch it:
```bash
blueman-manager
```

---

## Step 2: Put PS4 Controller in Pairing Mode

1. Disconnect any USB cables.
2. Press and hold **PS + Share** for ~6–7 seconds.
   - The light bar will blink **rapidly** (pairing mode)
3. Ensure your **PS4 console is off or unplugged** (it can steal the connection)

---

## Step 3: Pair and Trust in Blueman

1. In Blueman, click **Search**
2. Select **"Wireless Controller"**
3. Right-click → **Pair**
4. Right-click again → **Trust**
5. Right-click again → **Connect**

The controller’s light bar should turn **solid** — it’s connected 

---

## Step 4: Confirm the Connection

Run in terminal:
```bash
bluetoothctl
info <controller_mac>
```

MAC stands for **Media Access Control**. It’s a unique identifier assigned to every Bluetooth (or network) device — kind of like a digital fingerprint.
For example it could be: `AC:36:1B:93:81:B9`.


You should see:
```
Paired: yes
Trusted: yes
Connected: yes
```

---

## Step 5: Auto-Reconnect on Boot

1. Edit crontab:
```bash
crontab -e
```

2. Choose `nano` if prompted.

3. Add the line below to the bottom (replace MAC address):
```bash
@reboot sleep 10 && echo -e 'connect AC:36:1B:93:81:B9\nexit' | bluetoothctl
```

4. Save and exit (`Ctrl + O`, `Enter`, `Ctrl + X`)

---

## Step 6: Power-On & Shutdown Best Practices

**Before shutdown:**
- Hold **PS button** until controller turns off

**Then shutdown the Pi properly:**
```bash
sudo shutdown now
```

**When powering on:**
- Boot Pi
- Wait 10 seconds
- Press **PS** button to reconnect controller: You should see a solid light bar (if blinking, give it a second or press again)

---

## Optional: Manual Test Without Reboot

```bash
echo -e 'connect AC:36:1B:93:81:B9\nexit' | bluetoothctl
```

---

## Done!

You now have:
- Persistent pairing
- Automatic reconnection on boot
- A clean and reliable setup with no more re-pairing headaches
