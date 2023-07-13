# alsa-motu-m2
Some info about using the MOTU M2 audio interface with alsa on Linux.

## Info
Issues with the M2 / M4 seem to be caused by a couple different issues -
- `alsa-lib` / `lib32-alsa-lib` / `alsa-ucm-conf` / `alsa-utils` > 1.27.1-1
- ~~A [kernel bug](https://bugzilla.kernel.org/show_bug.cgi?id=216500) present in verisons 5.19.8 and newer~~ ([now fixed](https://bugzilla.kernel.org/attachment.cgi?id=301839&action=edit))
- Another kernel bug, affecting card detection at boot, requiring a power-cycle to fix (I've had luck [increasing msleep from 2000 to 4000](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/sound/usb/quirks.c?h=v5.15-rc1#n1128) to fix this)

These monkey-patch confs fix MOTU M2 / M4 audio issues specifically relating to the first point. 

## Install

<details><summary>Downgrade alsa-lib, lib32-alsa-lib, & alsa-ucm-conf to 1.2.7.1-1</code></summary>
<br />

- downgrade ([github](https://github.com/archlinux-downgrade/downgrade) / [AUR](https://aur.archlinux.org/packages/downgrade))
<br /></details>

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

