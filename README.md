# GIT Codebase

<br>

## Table of Contents GIT

- [GIT](#)
  - [Basic Rules](#basic-rules)
  - [Branching](#branching)
  - [Commits](#commits)
  - [Code Reviews](#code-reviews)
  - [Merge](#merge)
  - [Rebase](#rebase)
  - [Squash Commits](#squash-commits)

---

<br>

## Basic Rules

<b>Konfigurasi commit authorship</b><br>
Atur username dan email kamu dengan benar saat menggunakan Git. Informasi ini akan dilampirkan ke setiap `commit` yang kamu buat.

```bash

git config --global user.name "John Doe"

git config --global user.email "john@example.com"

```

Dengan cara ini rekan developer lain akan mudah menemukan kamu jika mereka memiliki pertanyaan tentang perubahan terkait `commit` milik kamu.

<br>

## Branching

```bash
git checkout -b branch_name/sub_name
```

**Development**: **_harus melalui pull request / jangan direct push/merge ke branch ini_**.
Setiap aggota tim harus mengerjakan task-nya pada branch yang terpisah dari upstream_branch. Hal ini akan memudahkan kamu dalam membuat perubahan tanpa mempengaruhi branch utama.
Ketika task yang dikerjakan dirasa sudah selesai, kamu dapat melakukan merge melalui pull request terlebih dahulu ke branch utama.

Branch list:

- master `default production branch` (<b>locked for push<b/>)
- release `testing branch form staging to production`
- develop `development branch` (<b>locked for push<b/>)
- staging `staging release branch`
- feature `new feature branch, pull from development`
- bugfix `bugfix branch, pull from development`
- hotfix `hotfix error production, pull from production`

List all the available branches

```bash
git branch -a
```

Create New Branch

```bash
git checkout -b <branch_name> <upstream_branch>
```

<b>Example usage</b>

bugfix

```bash
git checkout -b bugfix/[SUBTASK-ID] Development
```

hotfix

```bash
git checkout -b hotfix/[SUBTASK-ID] Development
```

feature

```bash
git checkout -b feature/[SUBTASK-ID] Development
```

release

```bash
git checkout -b release/[VERSIONING] Development
```

Format versioning x.y.z <br>
x == major version<br>
y == minor version<br>
z == patch version<br>

<br>

## Commits

Commit Messages sangat penting untuk memahami apa yang telah kamu kerjakan pada commit tersebut. Jadi luangkan waktu untuk menulis commit yang singkat tapi terperinci dan bermakna. `jangan lupa untuk menyematkan link task/story JIRA ya..`

## Structure commit message

```text
[section head of title]

[section dectription / summary]

[section taklist / substask]

[section link task / story]
```

## Example commit message

```text
Feature Banner slider API

  Added the Absorb Banner Slider feature for frontend usage needs
   - add model or repository Banner
   - add controller crud Banner
   - add serialize & validator Banner

   https://telkomdds.atlassian.net/browse/SCAN-1362
```

<br>

## Code Reviews

Ketika kamu melakukan `Pull Request`, untuk memastikan kualitas kode kamu wajib menyematkan reviewer. Review koding dilakukan agar kode yang kita buat bisa dinyatakan efektif. Meminta review juga memastikan bebrapa baris kode kamu tidak mengandung implikasi keamanan dan lain-lain.

<br>

## Merge

Merge dilakukan untuk menyatukan 2 branch dalam satu commit. Proses merge branch sebaiknya hanya di lakukan melalui pull request dan hanya menuju `upstream branch` atau branch diatasnya. Jika kita membuat branch baru atau mengupdate commit terakhir dengan referensi upstream branch (development), maka tidak boleh melakukan merge karena proses ini akan membuat node commit baru dan akan bermasalah dikemudian hari jika terjadi conflict dll. Proses yang harus dilakukan yaitu melalui prosedur `rebase`.

<br>

## Rebase

Kamu harus sering me-rebase branch milik kamu, untuk menghindari konflik dan kerja dua kali. berikut perintah untuk melakukan rebase:

```bash
git checkout <upstream_branch>

git pull

git checkout <your_branch>

git rebase <upstream_branch>
```

<br>

## Squash Commits

Pada saat mengerjakan task di branch milik kamu, sangat dimungkinkan untuk menambahkan komit sekecil apapun. Namunm jika setiap branch memiliki banyak commit, jumlah commit di branch utama seperti development, staging dll juga akan memiliki beban commit yang jumlahnya sama. Sebaiknya hanya ada satu commit dari setiab branch yang akan di merge ke branch utama.

Dalam proses ini, Kamu akan mengambil semua commit dengan perintah git rebase dengan flag i dan menyatukannya dengan squash. Selain menyederhanakan commit, perintah ini juga memungkinkan Anda untuk menghapus commit, menulis ulang pesan komit, dan menambahkan file baru.

**See ~N Commit**

```bash
git log --graph --oneline --decorate
```

Berikut adalah command squash, N adalah jumlah commits yang akan digabung menggunakan rebase:

```bash
git rebase -i HEAD~[N]
```

menjadi seperti ini:

```bash
git rebase -i HEAD~4
```

Setelah command tersebut, GIT akan memulai rebase dengan default editor (Vim editor):

```bash
pick c407062 Commit first commit
pick 383474d Commit Added css resets
pick b45h6js Commit third commit
pick kl3938n Commit Major commit
```

kamu dapat mengubah pick commit baris kedua dan seterusnya menjadi s atau squash, ini akan menyatukan beberapa commit menjadi satu.

```bash
pick c407062 Commit first commit
s 383474d Commit Added css resets
s b45h6js Commit third commit
s kl3938n Commit Major commit
```

jika sudah mengubah commit menjadi squash, silahkan tekan tombol escape dan ketik `:wq` untuk melanjutkan proses rebase, kemudian langkah selanjutnya adalah menyatukan commit message.

Pada editor commit message silahkan anda memilah, mengedit dan menghapus message jika diperlukan. Ketik `:wq` untuk melanjutkan proses rebase.

Berikut list command pada fitur rebase

```bash
Continue with the changes that you made.
  git rebase --continue

Skips the changes
  git rebase --skip

Abort rebase
  git rebase --abort
```
