# Phụ lục

- [Tổng quan](#tổng-quan)
    + [Tài liệu tham khảo](#tài-liệu-tham-khảo)
- [Làm việc với dự án](#làm-việc-với-dự-án)
    + [feature](#bắt-đầu-một-feature)
    + [release](#bắt-đầu-release-một-tính-năng)
    + [hotfix](#bắt-đầu-hotfix-một-version)
- [Tips & tricks](#các-tips--tricks-làm-việc-hiệu-quả-với-git)
- [Khi nào sử dụng rebase và merge](#4-khi-nào-sử-dụng-rebase-và-merge)
- [Copyright](#copy-right)


# Tổng quan.
## Tài liệu tham khảo
[gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
[git-flow-cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/)


## Làm việc với dự án

![alt text](images/git_flow.png)

### Bắt đầu một feature.

Lưu ý: Tài liệu này tuần theo phương pháp feature ngắn hạn. Có thể hiểu các task trên jira hoặc redmine là các feature nhỏ.
Có thể merge luôn vào develop để tránh việc giữ các feature dài hạn quá lâu và conflict.

Để giải quyết các vấn đề như các feature chưa được release vẫn ở trong branch develop sẽ sử dụng các kỹ thuật `Feature flag` hoặc release branch để xử lý.

**Step 1:** Thực hiện lấy thông tin task dựa trên redmine hoặc jira (công cụ quản lý dự án)
**Step 2:** Tạo branch mới từ `develop` branch.
```
git checkout develop
git checkout -b feature/#TASK_ID-<Mô tả gắn gọn về task>
```

Ví dụ:
git checkout -b feature/#1743-fix-filter-battery.

**Step 3:** Coding.
Thực hiện phát triển coding.
Sau khi thực hiện coding done thực hiện đẩy source lên github hoặc gitlab ... (bên thứ 3 quản lý source code online)

**Step 3.1:** Commit code
[Quy tắc đặt tên commit](git_commit_convention.md);

```
feat(1743 battery): Fix filter battery
{Diễn tả thêm về commit}
```

```
refactor(1743 battery): Refactor filter battery
{Diễn tả thêm về commit}
```

```
fix(1743 battery): Fix bug filter battery
{Diễn tả thêm về commit}
```

```
unittest(1743 battery): write unit test filter battery
{Diễn tả thêm về commit}
```

**Step 3.2:** Push code

```
git push origin <TÊN BRANCH>
```

*NOTE: Lưu ý hạn chế sử dụng flag -f (--force) vì nó sẽ làm mất lịch sử git.*

**Step 4:** Lên github tạo Pull Request
**Step 5:** Thực hiện check lại thay đổi source code của mình 1 lần nữa.
**Step 6:** Gửi cho leader hoặc quản lý review source.

Thực hiện check comment của quản lý và thực hiện sửa comment và đẩy lên lại branch của mình cho đến khi hoàn thành và được approval.

*Dành cho leader*
**Step 7:** Thực hiện merge code vào develop hoặc feature.
Sử dụng `squash commit` để merge đảm bảo rằng không có quá nhiều commit rác.

**Step 8:** Xoá branch
Thực hiện xoá branch sau khi merge

### Bắt đầu release một tính năng.
**Dành cho leader hoặc người có quyền release**

Sau khi việc **testing đã pass qua tester** để chuẩn bị cho việc release về mặt source code
Git flow để release một chức năng nào đó.

**Step 1:** Từ branch `develop` thực hiện tạo branch release.

```
git checkout release/v2.0.1

hoặc

git checkout release/v2.0.1-battery-feature

```
*github hiện đang không cho đặt là `release/` có thể sửa thành `release-`*

**Step 2:** Coding.
Thực hiện chuẩn bị cho việc release như:

- [x] Tắt các chức năng không được release.
- [x] Kiểm tra và review lại bug.

**Step 3:** Push code
Đẩy code lên github

**Step 4:** Tạo pull request
Tạo pull request vào `master`

**Step 5:** Merge
Thực hiện merge code vào `master`

*NOTE: Sử dụng `Create merge` không sử dụng `Squash merge` để đảm bảo rằng các commit được đồng bộ từ develop và master và tránh conflict commit khi thực hiện flow `hotfix`*

**Step 6:** Tạo tags
Tạo tags trên github trỏ vào branch `master` và đặt tag version tương ứng

**Step 7:** Merge code vào branch `develop`

**Step 8:** Xoá branch
Thực hiện xoá branch sau khi merge

### Bắt đầu hotfix một version
**Thường cho leader**

Khi bạn đã thực hiện release một chức năng hoặc đã tạo tags mà có vấn đề fix bug hoặc cần sửa đổi gì đó thì chúng ta cần một hotfix để thực hiện điều đó.

**Step 1:** Từ branch `master` thực hiện tạo branch `hotfix`.

```
git checkout master
git pull origin master # Update code
git checkout -b hotfix/fix-filter-battery
```

**Step 2:** Sau khi thực hiện fix xong thì bạn có thể tạo release luôn.

**Step 3:** Tạo pull request
- [x] Tạo pull request và merge vào master
- [x] Tạo pull request và merge vào develop
- [x] (Optional) Tạo pull request và merge vào các feature đang phát triển.

**Step 4:** Đặt tags
Thực hiện tạo tags trỏ vào branch `master` để cập nhật release version

**Step 5:** Xoá branch
Thực hiện xoá branch sau khi merge

## Các tips & tricks làm việc hiệu quả với git.

### 1. Một pull request hoặc 1 feature ngắn hạn chỉ nên từ 15 - 20 files change

- [x] Đảm bảo việc review source code dễ dàng.
- [x] Dễ dàng trong quá trình merge/rebase source code.

### 2. Mỗi pull request nên chỉ chỉ có 1 commit.

- [x] Thường các pull là một chức năng nhỏ nên việc tạo ra các commit thừa là không cần thiết.

### 3. Đặt tên branch hoặc commit.

- [x] Đặt tên branch và tên commit theo git convention quy định tuỳ thuộc vào từng công ty hoặc từng team dự án giúp làm việc với git hiệu quả hơn
- [x] Việc tìm kiếm commit trở lên đơn giản hơn.
- [x] Git tree trông chuyên nghiệp và clean hơn.

### 4. Khi nào sử dụng rebase và merge
Việc sử dụng rebase và merge gây khá nhiều tranh cãi. Tuy nhiên đây sẽ là 1 best practice các bạn nên tuân theo để làm việc với git dễ dàng hơn.

**Khi nào sử dụng rebase**
- [x] Cập nhật code từ nhánh chính `develop` vào các nhánh phụ `feature`, `hotfix` hoặc `release`

*Việc sử dụng rebase có 1 nhược điểm nó có thể `tạo lại (rehash)` lại mã commit. Nên thường nếu bạn dùng rebease thì cần sử dùng `push -f` để cập nhật code thay đổi lên github*

**Khi nào sử dụng merge**
- [x] Cập nhật code từ nhánh phụ `feature`, `hotfix` hoặc `release` vào nhánh gốc `develop`, `master`

Nhược điểm của merge sẽ sinh ra nhiều commit. Tuy nhiên nó sẽ dữ được lịch sử commit.

# Copy right
Name: Hà Văn Đức.
Email: duchv2008@gmail.com
Facebook: fb.com/nhocbangchu95