# nanopc-t6-vendor-bsp

## What is this

This is the **top-level meta repository** for the NanoPC-T6 (RK3588) vendor
BSP archive project. It ties together the kernel, U-Boot, and a set of
third-party drivers that FriendlyElec bundled into their board support
package without disclosing (or, in most cases, even naming) their real
upstream origin. Every component below is a `git submodule` pinned to a
specific, independently-verified upstream commit — nothing here is a raw
copy of source code.

## Motivation / GPL compliance background

FriendlyElec does not publish a direct download link for the NanoPC-T6
kernel/U-Boot/driver sources on their official website. They are only
reachable by manually navigating through their wiki into a nested Google
Drive ("netdisk") folder. This arguably satisfies the *letter* of GPLv2's
written-offer requirement, but in practice suppresses discoverability.

This project exists to:
1. Make the source available through a normal, indexable channel (plain git
   repos, searchable on GitHub) — see [`kernel/`](./kernel) and
   [`uboot/`](./uboot).
2. Identify the **real upstream** of the "additional drivers" FriendlyElec
   bundled alongside the kernel — several of these turned out not to be
   FriendlyElec's own work at all (see the component table below), and one
   (`rtl8822bu`) turned out to actually be FriendlyElec's own public GitHub
   repo, just never linked from the wiki/netdisk package.
3. Serve as a stable, versioned reference for driver/firmware/bootloader
   study on this hardware.

## Components

| Component | Path | Upstream | Pinned commit | Packaged/commit date | License | Notes |
|---|---|---|---|---|---|---|
| Kernel | [`kernel/`](./kernel) | [ithelpm/nanopc-t6-vendor-kernel](https://github.com/ithelpm/nanopc-t6-vendor-kernel) (this project's own archive of FriendlyElec's vendor kernel 6.1.55) | `6a98dc8b2` | 2023-10-19 (vendor commit `11e6fba`) | GPL-2.0 WITH Linux-syscall-note | Vendor tarball omitted the `LICENSES/` dir; substitute full GPLv2 text added |
| U-Boot | [`uboot/`](./uboot) | [ithelpm/nanopc-t6-vendor-uboot](https://github.com/ithelpm/nanopc-t6-vendor-uboot) (this project's own archive of FriendlyElec's vendor U-Boot v2017.09) | `58a3255` | 2023-08-08 (vendor commit `22147f3`) | GPL-2.0-or-later | |
| rtl8812au | `drivers-extra/rtl8812au` | [aircrack-ng/rtl8812au](https://github.com/aircrack-ng/rtl8812au) | `33b3e698c297c998b00a7693c49101c64d3ed437` | 2023-09-18 | GPL | |
| rtl8822bu | `drivers-extra/rtl8822bu` | [friendlyarm/rtl8822bu](https://github.com/friendlyarm/rtl8822bu) | `91db736ab7307293e605e9c3899f5c4eed40272f` | 2023-10-13 | GPL | **This is FriendlyElec's own public repo**, not a third party — the vendor snapshot just never linked to it |
| rtw88 | `drivers-extra/rtw88` | [lwfinger/rtw88](https://github.com/lwfinger/rtw88) (standalone backport package, not mainline Linux) | `0208fe971e92322ac13db18a2395847c5ee896f4` | 2023-10-18 | GPL / dual BSD-GPL | Confirmed not present in `torvalds/linux` |
| cryptodev-linux | `drivers-extra/cryptodev-linux` | [cryptodev-linux/cryptodev-linux](https://github.com/cryptodev-linux/cryptodev-linux) | `bb8bc7cf60d2c0b097c8b3b0e807f805b577a53f` | 2023-07-03 | GPLv2 (per local `COPYING`) | |
| nft-fullcone | `drivers-extra/nft-fullcone` | [fullcone-nat-nftables/nft-fullcone](https://github.com/fullcone-nat-nftables/nft-fullcone) (repo archived, still fetchable) | `07d93b626ce5ea885cd16f9ab07fac3213c355d9` | 2023-05-17 | GPL | |
| r8125 | *not yet included* | **unresolved** — checked 20+ community mirrors/DKMS packages, none contain the vendor snapshot's commit (`0551562`) | — | — | `MODULE_LICENSE("GPL")` (source-declared; no LICENSE/COPYING file in the vendor snapshot) | Pending a decision on standalone archival; not wired in as a submodule yet |

## Download / acquisition channel

All components trace back to FriendlyElec's NanoPC-T6 wiki → netdisk
(Google Drive folder navigation), with no direct public download URL
published on their official site. See each submodule's own README for its
specific original filename, download date, and SHA256.

## License overview

Every submodule retains its own upstream license — see the table above and
each submodule's own `LICENSE`/`COPYING` file. In short: everything here is
GPL-licensed in some form (GPL-2.0-only, GPL-2.0-or-later, or dual
BSD/GPL-2.0 depending on the component); this meta repository adds no
additional license terms of its own — it is purely an organizational
wrapper (`.gitmodules` + documentation).

## Disclaimer

This is an **unofficial archival and documentation project**. It is not
maintained by, and not affiliated with, FriendlyElec or any of the upstream
projects listed above. Provided as-is, with no warranty of any kind, and no
guarantee that it will track or stay in sync with any future FriendlyElec
BSP releases.

---

## 這是什麼

這是 NanoPC-T6（RK3588）vendor BSP 封存專案的**頂層 meta repo**。透過 git
submodule 把 kernel、U-Boot，以及一批友善電子（FriendlyElec）打包進 BSP、卻
沒有揭露（多數情況下甚至沒有具名）真實 upstream 出處的第三方驅動串在一起。
下面表格裡的每一項都是 pin 到特定、已獨立驗證過的 upstream commit 的 git
submodule——這裡沒有任何一份是重新上傳的原始碼副本。

## 動機／GPL 合規性背景

友善電子並未在官網公開 NanoPC-T6 kernel/U-Boot/驅動原始碼的直接下載連結，
必須透過 wiki 頁面逐層點進 Google Drive（netdisk）資料夾才找得到。這樣的做法
在形式上或許符合 GPLv2「書面要約」的字面要求，但實質上刻意降低了可見度。

這個專案的目的是：
1. 讓原始碼可以透過一般、可被搜尋索引的管道（公開的 git repo）取得——見
   [`kernel/`](./kernel) 與 [`uboot/`](./uboot)。
2. 找出友善電子打包進 kernel 旁邊那批「額外驅動」的真實 upstream——其中好幾個
   根本不是友善電子自己的東西（見下方元件對照表），而其中一個（`rtl8822bu`）
   反而查出來就是友善電子自己在 GitHub 上公開的 repo，只是 wiki/netdisk 打包
   時完全沒有附上連結。
3. 作為研讀這款硬體 driver/firmware/bootloader 的穩定、有版本紀錄的參考素材。

## 元件對照表

（內容同上方英文表格：kernel/uboot 對應各自的封存 repo 與打包日期；
`rtl8812au`、`rtw88`、`cryptodev-linux`、`nft-fullcone` 皆為已確認的第三方
upstream 並 pin 特定 commit；`rtl8822bu` 特別註明其 upstream 其實是友善電子
自己的公開 repo；`r8125` 目前尚未找到對應 upstream，暫不收錄進本 repo，留待
後續決定是否獨立封存。）

## 下載／取得管道

所有元件都可追溯回友善電子 NanoPC-T6 wiki → netdisk（Google Drive 逐層瀏覽），
官網並未公開直接下載連結。各自的原始檔名、下載日期、SHA256 請見各 submodule
自己的 README。

## 授權總覽

每個 submodule 都保留自己 upstream 的授權條款——詳見上方表格與各 submodule
自帶的 `LICENSE`/`COPYING`。簡單說：這裡的東西都是某種形式的 GPL 授權
（GPL-2.0-only、GPL-2.0-or-later，或依元件而定的 dual BSD/GPL-2.0）；這個
meta repo 本身不額外附加任何授權條款——純粹是組織性質的包裝（`.gitmodules` +
文件說明）。

## 免責聲明

這是一份**非官方的封存與文件整理專案**，並非友善電子或上述任何 upstream 專案
維護，也與它們無關。本 repo 不提供任何保證，且不保證會持續追蹤或同步友善電子
未來釋出的 BSP 版本。
