# ✨ دليلي الشخصي لإعداد فيدورا لينكس (بعد التثبيت)

<div dir="rtl">

<p align="center">
  <img src="src/assets/logo.png" alt="شعار إعداد فيدورا - طراز نورد" width="250"/>
</p>

أهلاً وسهلاً! يعني خلاص ثبتت فيدورا وقاعد تبص في الشاشة وتقول "إيه اللي بعد كده؟" كنت مكانك قبل كده، وصراحة إعداد نظام لينكس جديد ممكن يكون مربك شوية. علشان كده عملت الدليل ده - هو كل حاجة كنت نفسي حد يقولهالي لما بدأت أستخدم فيدورا أول مرة.

ده مش دليل رسمي أو حاجة. دي تجربتي الشخصية - إيه اللي شغال، إيه اللي بيكسر، وإزاي تصلحه لما الحاجات تخرب (وهتخرب، وده طبيعي خالص).

**الدليل ده لمين؟**
- جديد على فيدورا خالص؟ تمام كده.
- جاي من أوبونتو أو توزيعة تانية؟ برضه تمام.
- بتستخدم فيدورا من سنين بس عايز checklist كويسة؟ أيوه، انت كمان.
- مبتدئ في لينكس خالص؟ ممكن تتعب شوية، بس استحمل معانا!

**ملحوظة مهمة عن الأوامر:**
- لما تشوف `sudo` في الأول، يعني "شغل كأدمن" - هيطلب كلمة السر
- الـ `-y` يعني "أيوه لكل حاجة" علشان متفضلش تدوس enter
- النسخ واللصق صديقك، بس اقرا الأمر الأول قبل ما تشغله

---

## 📋 هنتكلم عن إيه

1. [🔥 أول حاجات لازم تعملها](#-أول-حاجات-لازم-تعملها)
   - [RPM Fusion - الحاجات الحلوة](#rpm-fusion---الحاجات-الحلوة)
   - [التحديثات (مملة بس مهمة)](#التحديثات-مملة-بس-مهمة)
   - [تحديثات الفيرم وير](#تحديثات-الفيرم-وير)
   - [خلي للكمبيوتر اسم](#خلي-للكمبيوتر-اسم)
2. [📦 تحميل برامج أكتر](#-تحميل-برامج-أكتر)
   - [إعداد Flathub](#إعداد-flathub)
   - [مستودع Terra](#مستودع-terra-لو-حاسس-إنك-مغامر)
3. [🎮 تعريفات كروت الشاشة](#-تعريفات-كروت-الشاشة)
   - [NVIDIA (الصعب)](#nvidia-الصعب)
   - [AMD وIntel (السهلين)](#amd-وintel-السهلين)
4. [🎵 خلي الميديا تشتغل](#-خلي-الميديا-تشتغل)
   - [كودكس الفيديو](#كودكس-الفيديو-علشان-كله-يشتغل)
   - [تسريع الهاردوير](#تسريع-الهاردوير)
   - [إصلاح فيديو فايرفوكس](#إصلاح-فيديو-فايرفوكس)
5. [🔧 حاجات مفيدة](#-حاجات-مفيدة)
   - [دعم الأرشيف](#دعم-الأرشيف)
   - [خطوط مايكروسوفت](#خطوط-مايكروسوفت-للأسف-لسه-محتاجينها)
   - [دعم AppImage](#دعم-appimage)
6. [⚡ خلي الحاجات أسرع](#-خلي-الحاجات-أسرع)
   - [إقلاع أسرع](#إقلاع-أسرع)
   - [بطارية تعيش أكتر](#بطارية-تعيش-أكتر)
   - [إصلاح وقت الـ Dual Boot](#إصلاح-وقت-الـ-dual-boot)
   - [تحسينات الأداء](#تحسينات-الأداء-خلي-بالك)
7. [🔒 حاجات الأمان](#-حاجات-الأمان)
   - [DNS مشفر](#dns-مشفر-اختياري-بس-حلو)
8. [💾 اعمل backup لحاجاتك](#-اعمل-backup-لحاجاتك)
   - [لقطات النظام](#لقطات-النظام)
   - [الملفات الشخصية](#الملفات-الشخصية)
9. [🎮 إعداد الألعاب](#-إعداد-الألعاب)
   - [Steam والألعاب](#steam-والألعاب)
10. [🌟 البرامج اللي فعلاً بستخدمها](#-البرامج-اللي-فعلاً-بستخدمها)
    - [المتصفحات](#المتصفحات)
    - [البرمجة](#البرمجة)
    - [شغل المكتب](#شغل-المكتب)
11. [🧹 خلي كله نضيف](#-خلي-كله-نضيف)
    - [شيل الحاجات الزيادة](#شيل-الحاجات-الزيادة)
    - [تنضيف النظام](#تنضيف-النظام)
12. [👏 شكر وتقدير](#شكر-وتقدير)

---

## 🔥 أول حاجات لازم تعملها

### RPM Fusion - الحاجات الحلوة

طيب، أول حاجة - فيدورا بتيجي عارية شوية بسبب مشاكل قانونية. RPM Fusion هو المكان اللي فيه كل الحاجات المفيدة فعلاً (الكودكس، التعريفات، إلخ). انت محتاجه.

</div>

```bash
# جيب المستودع المجاني (معظم الحاجات اللي محتاجها)
sudo dnf install -y \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# جيب المستودع غير المجاني (تعريفات NVIDIA، كودكس معينة)
sudo dnf install -y \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

# حدث كله علشان كله يشتغل مع بعض
sudo dnf group upgrade core
sudo dnf check-update
```

<div dir="rtl">

### التحديثات (مملة بس مهمة)

أيوه، أعرف إن التحديثات مملة. بس جد كده، اعمل كده الأول. التثبيت الجديد دايماً فيه packages قديمة.

</div>

```bash
# حدث كل حاجة
sudo dnf update -y

# لو حدث الكيرنل، اعمل ريستارت
sudo reboot
```

<div dir="rtl">

### تحديثات الفيرم وير

الهاردوير بتاعك على الأغلب فيه فيرم وير أحدث متاح. ده مهم فعلاً لحاجات زي الواي فاي وعمر البطارية.

</div>

```bash
# شوف إيه اللي ممكن يتحدث
sudo fwupdmgr get-devices

# حدث قاعدة بيانات الفيرم وير
sudo fwupdmgr refresh --force

# دور على تحديثات
sudo fwupdmgr get-updates

# طبقهم
sudo fwupdmgr update
```

<div dir="rtl">

بعض تحديثات الفيرم وير محتاجة ريستارت. اعمله خلاص.

### خلي للكمبيوتر اسم

ده علشان الشكل بس بس هيخليك تحس إنك في البيت. اختار حاجة حلوة!

</div>

```bash
# بدل باللي انت عايزه
sudo hostnamectl set-hostname my-awesome-fedora-box
```

<div dir="rtl">

## 📦 تحميل برامج أكتر

### إعداد Flathub

فيدورا بتيجي بنسخة مخصية من Flatpak. Flathub هو المكان اللي فيه البرامج الحقيقية.

</div>

```bash
# شيل المستودع المحدود بتاع فيدورا
flatpak remote-delete fedora

# ضيف Flathub الحقيقي
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# حدث كل حاجة
flatpak update --appstream
```

<div dir="rtl">

### مستودع Terra (لو حاسس إنك مغامر)

Terra فيه شوية packages من الكومينيتي مش موجودة في المستودعات الأساسية. اختياري بس أحياناً مفيد.

</div>

```bash
sudo dnf install -y --nogpgcheck --repofrompath 'terra,https://repos.fyralabs.com/terra$releasever' terra-release
```

<div dir="rtl">

## 🎮 تعريفات كروت الشاشة

### NVIDIA (الصعب)

NVIDIA على لينكس... معقد شوية. ده بيشتغل معظم الوقت، بس لو عندك مشاكل، أهلاً بيك في النادي.

**قبل ما تبدأ:**
- قفل Secure Boot في الـ BIOS (أو اتعلم تعمل sign للكيرنل modules - انت حر)
- اتأكد إن كل حاجة محدثة ومعمولها ريستارت

</div>

```bash
# ثبت أدوات الـ build
sudo dnf install -y kernel-devel kernel-headers gcc make dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig

# لو عندك RTX 40 series أو أحدث، فعل open kernel modules
sudo sh -c 'echo "%_with_kmod_nvidia_open 1" > /etc/rpm/macros.nvidia-kmod'

# ثبت التعريفات
sudo dnf install -y akmod-nvidia xorg-x11-drv-nvidia-cuda

# دلوقتي استنى. جد كده، ده بياخد 5-15 دقيقة
# ممكن تتابع التقدم بـ: sudo journalctl -f -u akmods

# اعمل ريستارت لما يخلص
sudo reboot

# شوف لو اشتغل
nvidia-smi
```

<div dir="rtl">

**لو مش شغال:**

</div>

```bash
# اجبر rebuild وحاول تاني
sudo akmods --force --kernels $(uname -r)
sudo reboot
```

<div dir="rtl">

### AMD وIntel (السهلين)

دول عادة بيشتغلوا لوحدهم، بس خلينا نخليهم يشتغلوا أحسن.

</div>

```bash
# التعريفات الأساسية ودعم Vulkan
sudo dnf install -y mesa-dri-drivers mesa-vulkan-drivers vulkan-loader mesa-libGLU

# تسريع فيديو AMD (بيخلي الفيديوهات أنعم)
sudo dnf install -y mesa-va-drivers-freeworld mesa-vdpau-drivers-freeworld

# تسريع فيديو Intel (لكروت Intel الأحدث)
sudo dnf install -y intel-media-driver
```

<div dir="rtl">

## 🎵 خلي الميديا تشتغل

### كودكس الفيديو (علشان كله يشتغل)

فيدورا بتيجي من غير كودكس تقريباً بسبب مشاكل براءات الاختراع. ده بيحل الموضوع.

</div>

```bash
# استبدل ffmpeg المخصي بالحقيقي
sudo dnf swap -y ffmpeg-free ffmpeg --allowerasing

# ثبت كل plugins الـ GStreamer
sudo dnf install -y gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav lame\* --exclude=gstreamer1-plugins-bad-free-devel

# ثبت مجموعات الملتيميديا
sudo dnf groupupdate -y multimedia --setopt="install_weak_deps=False" --allowerasing
sudo dnf groupupdate -y sound-and-video --allowerasing
```

<div dir="rtl">

### تسريع الهاردوير

ده بيخلي تشغيل الفيديو يستخدم كارت الشاشة بدل ما يدق على المعالج.

</div>

```bash
# ثبت حاجات VA-API
sudo dnf install -y ffmpeg-libs libva libva-utils

# لو عندك NVIDIA، ضيف ده كمان
sudo dnf install -y nvidia-vaapi-driver
```

<div dir="rtl">

### إصلاح فيديو فايرفوكس

فايرفوكس محتاج مساعدة شوية مع فيديوهات H.264.

</div>

```bash
# ثبت كودك Cisco (مجاني بس الترخيص غريب)
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264

# فعل مستودع Cisco
sudo dnf config-manager --set-enabled fedora-cisco-openh264
sudo dnf update -y
```

<div dir="rtl">

اعمل ريستارت لفايرفوكس واتأكد إن plugin الـ OpenH264 مفعل في `about:addons`.

## 🔧 حاجات مفيدة

### دعم الأرشيف

علشان أكيد هتحتاج تفك ملف .rar يوم من الأيام.

</div>

```bash
sudo dnf install -y p7zip p7zip-plugins unrar
```

<div dir="rtl">

### خطوط مايكروسوفت (للأسف لسه محتاجينها)

صفحات الويب والمستندات لسه بتبان وحش من غير الخطوط دي.

</div>

```bash
# ثبت الحاجات المطلوبة
sudo dnf install -y curl cabextract xorg-x11-font-utils fontconfig

# ثبت الخطوط
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm

# حدث كاش الخطوط
sudo fc-cache -fv
```

<div dir="rtl">

### دعم AppImage

شوية برامج بتيجي AppImages بس. ده بيخليهم يشتغلوا.

</div>

```bash
# ثبت FUSE
sudo dnf install -y fuse libfuse2

# اختياري: مدير AppImage (مفيد فعلاً)
flatpak install -y flathub it.mijorus.gearlever
```

<div dir="rtl">

## ⚡ خلي الحاجات أسرع

### إقلاع أسرع

ده سهل وبيعمل فرق واضح.

</div>

```bash
sudo systemctl disable NetworkManager-wait-online.service
```

<div dir="rtl">

### بطارية تعيش أكتر

إدارة الطاقة في فيدورا كويسة، بس لو عايز تحكم أكتر:

</div>

```bash
# ثبت TLP (بس لو محتاجه)
sudo dnf install -y tlp tlp-rdw

# فعله
sudo systemctl enable --now tlp
sudo systemctl mask power-profiles-daemon.service

# لتحليل استهلاك الطاقة
sudo dnf install -y powertop
```

<div dir="rtl">

ثبت TLP بس لو إدارة الطاقة المدمجة في فيدورا مش شغالة معاك.

### إصلاح وقت الـ Dual Boot

لو عندك dual boot مع ويندوز، ده بيصلح موضوع إن الساعة بتبقى غلط.

</div>

```bash
# قول لفيدورا يستخدم UTC للساعة
sudo timedatectl set-local-rtc 0 --adjust-system-clock
```

<div dir="rtl">

### تحسينات الأداء (خلي بالك)

**⚠️ تحذير:** ده بيقفل حاجات أمان مهمة. اعمله بس لو عارف بتعمل إيه ومحتاج الأداء فعلاً.

</div>

```bash
# لقفل CPU mitigations (مش مُوصى بيه)
# sudo grubby --update-kernel=ALL --args="mitigations=off"

# علشان تفعلهم تاني (اعمل ده)
# sudo grubby --update-kernel=ALL --remove-args="mitigations=off"
```

<div dir="rtl">

## 🔒 حاجات الأمان

### DNS مشفر (اختياري بس حلو)

ده بيشفر استعلامات DNS بتاعتك علشان مزود الإنترنت ميشوفش إيه المواقع اللي بتدخل عليها.

</div>

```bash
# ثبت dnscrypt-proxy
sudo dnf install -y dnscrypt-proxy

# اضبطه
sudo bash -c 'cat > /etc/dnscrypt-proxy/dnscrypt-proxy.toml <<EOL
listen_addresses = ["127.0.0.1:53", "[::1]:53"]
server_names = ["cloudflare"]
doh_servers = true
dnscrypt_servers = false
cache_size = 512
cache_min_ttl = 2400
cache_max_ttl = 86400
EOL'

# فعله
sudo systemctl enable --now dnscrypt-proxy.service

# اضبط systemd-resolved
sudo mkdir -p /etc/systemd/resolved.conf.d
sudo bash -c 'cat > /etc/systemd/resolved.conf.d/00-dnscrypt.conf <<EOL
[Resolve]
DNS=127.0.0.1#53 ::1#53
DNSSEC=allow-downgrade
Domains=~.
EOL'

# اعمل ريستارت للخدمات
sudo systemctl restart systemd-resolved.service

# قول لـ NetworkManager يبعد عن الطريق
sudo bash -c 'cat > /etc/NetworkManager/conf.d/01-dnscrypt-proxy.conf <<EOL
[main]
dns=none
systemd-resolved=false
EOL'

sudo systemctl restart NetworkManager
sudo systemctl restart dnscrypt-proxy.service

# اختبره
dig A example.com @127.0.0.1
```

<div dir="rtl">

## 💾 اعمل backup لحاجاتك

### لقطات النظام

لو بتستخدم Btrfs (الافتراضي في فيدورا الأحدث)، ممكن تاخد snapshots.

</div>

```bash
sudo dnf install -y btrfs-assistant btrbk snapper
```

<div dir="rtl">

الإعداد:
1. شغل `btrfs-assistant` علشان تعد snapshots بواجهة رسومية
2. فعل snapshots أوتوماتيكية:

</div>

```bash
sudo systemctl enable --now snapper-timeline.timer
sudo systemctl enable --now snapper-cleanup.timer
```

<div dir="rtl">

**مهم:** ده بيعمل backup لملفات النظام بس، مش حاجاتك الشخصية إلا لو ظبطته مختلف.

### الملفات الشخصية

استخدم Déjà Dup للمستندات والصور وكده.

</div>

```bash
# ثبته
sudo dnf install -y deja-dup

# أو جيبه من Flathub
flatpak install -y flathub org.gnome.DejaDup
```

<div dir="rtl">

**دايماً اعمل backup على تخزين خارجي أو cloud، مش نفس الهارد.**

## 🎮 إعداد الألعاب

### Steam والألعاب

Steam بيشتغل عالي على فيدورا بفضل Proton.

</div>

```bash
sudo dnf install -y steam
```

<div dir="rtl">

كده خلاص. جد كده. معظم ألعاب ويندوز بتشتغل دلوقتي.

## 🌟 البرامج اللي فعلاً بستخدمها

### المتصفحات

**Brave** (بيبلوك الإعلانات، سريع):

</div>

```bash
sudo dnf config-manager --add-repo https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo
sudo rpm --import https://brave-browser-rpm-release.s3.brave.com/brave-core.asc
sudo dnf install -y brave-browser
```

<div dir="rtl">

**Vivaldi** (خيارات تخصيص كتيرة):

</div>

```bash
sudo dnf config-manager --add-repo https://repo.vivaldi.com/archive/vivaldi-fedora.repo
sudo rpm --import https://repo.vivaldi.com/archive/linux_signing_key.pub
sudo dnf install -y vivaldi-stable
```

<div dir="rtl">

### البرمجة

**VS Code:**

</div>

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf install -y code
```

<div dir="rtl">

**Git:**

</div>

```bash
sudo dnf install -y git
```

<div dir="rtl">

**الحاويات (Podman أحسن من Docker، قاتلني):**

</div>

```bash
sudo dnf install -y podman podman-compose podman-docker
```

<div dir="rtl">

### شغل المكتب

**LibreOffice** (على الأغلب مثبت من قبل كده):

</div>

```bash
sudo dnf install -y libreoffice-writer libreoffice-calc libreoffice-impress libreoffice-draw
```

<div dir="rtl">

**OnlyOffice** (توافق أحسن مع MS Office):

</div>

```bash
sudo dnf install -y https://download.onlyoffice.com/install/desktop/editors/linux/onlyoffice-desktopeditors.x86_64.rpm
```

<div dir="rtl">

## 🧹 خلي كله نضيف

### شيل الحاجات الزيادة

**خلي بالك هنا.** شيل الحاجات اللي متأكد إنك مش محتاجها بس.

</div>

```bash
# شوف إيه المثبت
dnf list installed | less

# شوف إيه اللي معتمد على package قبل ما تشيله
dnf info package-name
dnf repoquery --whatrequires package-name

# أمثلة شيل (غير حسب احتياجك)
# sudo dnf remove -y cheese rhythmbox
```

<div dir="rtl">

### تنضيف النظام

اعمل ده من وقت للتاني علشان تخلي مساحة.

</div>

```bash
# نضف كاش الـ packages
sudo dnf clean all

# شيل packages اليتيمة
sudo dnf autoremove -y

# شيل kernels القديمة (لو عندك كتير)
# sudo dnf remove $(dnf repoquery --installonly --latest-limit=-2 -q)
```

<div dir="rtl">

---

## 👏 شكر وتقدير

الدليل ده موجود علشان ناس كتير قبلي اكتشفوا الحاجات دي وشاركوها. شكر كبير لناس زي devangshekhawat وpaulsorensen وMohammed Besar، وناس كتيرة في المنتديات وreddit ساعدوا الناس تخلي فيدورا تشتغل صح.

*ده مش دليل فيدورا الرسمي - ده اللي شغال معايا بس. ممكن معاك يبقى مختلف. دايماً اعمل backup للحاجات المهمة قبل ما تعمل تغييرات كبيرة، ومتلومنيش لو حاجة اتكسرت (بس متتردش تطلب مساعدة في منتديات فيدورا).*

---

**ملحوظة أخيرة:** لينكس ممكن يكون محبط أحياناً، بس هو كمان مُرضي جداً لما تظبطه زي ما انت عايز. متستسلمش لو حاجة مش شغالة من أول مرة - ده جزء من التجربة!

</div>
