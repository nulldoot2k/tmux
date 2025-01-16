# Tmux

Tmux là một terminal multiplexer, tức là một công cụ cho phép chúng ta chạy và quản lý nhiều phiên làm việc trong cùng một cửa sổ terminal. Điều này rất hữu ích khi chúng ta làm việc trên nhiều tác vụ hoặc muốn duy trì các phiên làm việc lâu dài mà không sợ mất tiến trình khi bị ngắt kết nối.

- Chạy với nhiều session trong cùng một cửa sổ terminal.
- Chia cửa sổ thành nhiều phần.
- Tạo các sesion và cửa sổ làm việc độc lập.
- Chạy các ứng dụng mà không lo bị mất dữ liệu khi kết nối lại.

Trong tmux, prefix là một tổ hợp phím đặc biệt mà bạn cần nhấn trước khi thực hiện các lệnh của tmux. Mặc định, tổ hợp phím prefix trong tmux là Ctrl+b.

Nhưng trong bộ cài đặt này sẽ sử dụng Ctr_a thay thế cho Ctr_b qua thiết lập sau

```
unbind C-b
set -g prefix C-a
bind C-a send-prefix
```

## Cài đặt - Installation

```bash
sudo apt-get install tmux

cat << EOF > ~/.tmux.conf
source-file ~/.config/tmux/tmux.conf
EOF
```

Thiết lập nhanh

```bash'
git clone https://github.com/nulldoot2k/tmux.git ~/.config/tmux
tmux
```

## Phím tắt - keyword

Để bắt đầu một phiên làm việc với tmux, chúng ta chỉ cần gõ: **tmux**

Tạo phiên (session) mới
```
tmux new-session -s session_name
```
Liệt kê các phiên đang chạy
```
tmux list-sessions
```
Chuyển giữa các phiên
```
tmux switch -t session_name or tmux ls
```

Xóa 1 phiên session

```
tmux kill-session -t <session>
```

Tách khỏi phiên
```
<prefix> + d
```

Kết thúc một phiên

```
exit or Ctr + d
```

Tạo cửa sổ mới

```
<prefix> + c
```

Di chuyển giữa các window

```
<prefix> + n  (next window)
<prefix> + p  (previous window)
or
Alt(hold) + <number>(1...9)
```

Chia cửa sổ theo chiều dọc:

```
<prefix> + " or <prefix> + \
```

Chia cửa sổ theo chiều ngang:

```
<prefix> + % or <prefix> + -
```

Chi tiết

```
bind '\' split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
```

Di chuyển giữa các pane

```
<prefix> + <arrow key> or <prefix> + <j,k,h,l>
```

Chi tiết

```
# Di chuyển giữa các pane, Không sử dụng Alt để di chuyển sang các pane do có sự trùng lặp với nvim
bind H select-pane -L    # Di chuyển sang pane bên trái khi nhấn Ctrl+b H
bind J select-pane -D    # Di chuyển sang pane dưới khi nhấn Ctrl+b J
bind K select-pane -U    # Di chuyển sang pane trên khi nhấn Ctrl+b K
bind L select-pane -R    # Di chuyển sang pane bên phải khi nhấn Ctrl+b L
```

Tăng size cho từng pane

```
Ctr_Alt(hold) + <j,k,h,l>
```

Chi tiết

```
# bạn có thể dùng Alt + h/j/k/l để điều chỉnh kích thước của pane
bind -n C-M-h resize-pane -L 2    # Thu nhỏ pane theo chiều ngang (trái)
bind -n C-M-j resize-pane -D 2    # Mở rộng pane theo chiều dọc (xuống)
bind -n C-M-k resize-pane -U 2    # Mở rộng pane theo chiều dọc (lên)
bind -n C-M-l resize-pane -R 2    # Mở rộng pane theo chiều ngang (phải)
```

Đổi tên cửa sổ

```
<prefix> + ,
```

Hiển thị danh sách các cửa sổ

```
<prefix> + w
```

Chia lại cửa sổ

```
<prefix> + q
```

Thay đổi cài đặt cho tmux

```
<prefix> + S
```

Zoom vào pane hiện tại, làm cho nó chiếm toàn bộ cửa sổ.

```
<prefix> z
```

Đồng bộ hóa các pane trong một cửa sổ

```
<prefix> Ctr_y
```

Chi tiết

```
# Chạy prefix sau đó giữ Ctr nhấn phím y
bind C-Y set-window-option synchronize-panes
```

Sao chép và dán nội dung trên Console

```
<prefix> [ y
```

Chi tiết

```
Để vào chế độ copy-mode-vi, bạn có thể nhấn <prefix> rồi nhấn [
Chế độ này cho phép bạn di chuyển con trỏ (sử dụng các phím vi như h, j, k, l), và chọn văn bản.
Sau khi chọn xong văn bản, bạn có thể nhấn y (theo cấu hình trên) để sao chép văn bản đã chọn.
Khi bạn nhấn y, tmux sẽ sao chép nội dung đã chọn và hủy chế độ copy-mode (thoát khỏi chế độ chọn).
Cuối cùng để dán nội dung đã sao chép, bạn có thể nhấn <prefix> rồi nhấn ] để dán nội dung vào nơi con trỏ hiện tại.
Lưu ý:
- Chỉ hoạt động trên terminal tmux console.
- Có thể dùng con lăn chuột bôi đen sau đó phím tắt 'y' để sao chép và <prefix> ] để paste.
```

## Connect Remote SSH
1. Client add public-key to Server

2. Server create session

```
tmux new-session -s session_name
```

3. Client connect to window tmux of Server

```
ssh <hostname> -t tmux <session_of_server>
```
