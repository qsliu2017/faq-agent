# List open files for a process on Unix

Inspect which files a process keeps open with `lsof` or the `/proc` filesystem.

## Use lsof for a single process

```bash
# Show every file descriptor held by PID 1234
lsof -p 1234

# Include network sockets and filter to regular files in /var/log
lsof -p 1234 +D /var/log
```

Run `sudo lsof -p "$PID"` if you get "no such process" or "operation not permitted".
Key columns include `FD` (file descriptor), `TYPE` (REG, CHR, IPv4), and `NAME` (path or socket description).
Add `+r 1` to keep refreshing every second when you are watching an active process.

```bash
# Continuously watch which files PID 1234 opens or closes
lsof -p 1234 +r 1
```

## Read open descriptors from /proc

If `lsof` is unavailable, list the file descriptor directory that Linux exposes for each PID.

```bash
ls -l /proc/1234/fd
```

Each entry is a symlink that points to the file, device, pipe, or socket backing the descriptor.
The `/proc/<pid>/fdinfo/<fd>` files include the offset and flags, which helps when auditing writes.

```bash
cat /proc/1234/fdinfo/3
```

Use `sudo` to read descriptors owned by another user.

## Related commands

`lsof -u alice` lists open files for every process owned by user `alice`.
`fuser /path/to/file` reports which processes currently have that path open.
