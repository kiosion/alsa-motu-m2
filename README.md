# alsa-motu-m2

hese monkey-patch confs fix MOTU M2 / M4 audio issues under Alsa on Linux (tested on Arch).

### Install

<details><summary>Move <a href="src/MOTU"><code>src/MOTU/*</code></a> to <code>/usr/share/alsa/ucm2/USB-Audio/MOTU/*</code></summary>
<p>

```diff
/usr/share/alsa/ucm2/USB-Audio/MOTU/M4.conf
/usr/share/alsa/ucm2/USB-Audio/MOTU/M4-HiFi.conf
+/usr/share/alsa/ucm2/USB-Audio/MOTU/M2.conf
+/usr/share/alsa/ucm2/USB-Audio/MOTU/M2-HiFi.conf
```

</p>
</details>
<details><summary>Replace the existing <code>/usr/share/alsa/ucm2/USB-Audio/USB-Audio.conf</code>'s MOTU M4 config with the updated config from <a href="src/USB-Audio.conf"><code>src/USB-Audio.conf</code></a></summary>
<p>

```diff
@@ -67,1 +83,1 @@
-If.M4 {
-	Condition {
-		Type String
-		Haystack "${CardComponents}"
-		Needle "USB07fd:000b"
-	}
-	True.Define.ProfileName "MOTU/M4"
+If.Motu {
+  Condition {
+    Type String
+    Haystack "${CardComponents}"
+    Needle "USB07fd:000b"
+  }
+  True.If.M4 {
+    Condition {
+      Type String
+      Haystack "${CardLongName}"
+      Needle "MOTU M4"
+    }
+    True.Define.ProfileName "MOTU/M4"
+    False.Define.ProfileName "MOTU/M2"
+  }
}
```

</p>
</details>

