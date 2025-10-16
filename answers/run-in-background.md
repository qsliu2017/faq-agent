# Run the current process in the background on Linux

When a command is already running in the foreground, suspend it, then resume in the background.

## Suspend, background, and manage jobs

```bash
# 1) Suspend the foreground job (sends SIGTSTP)
Ctrl+Z

# 2) Put the most recent suspended job in the background
bg

# 3) List jobs and bring one back to the foreground
jobs
fg %1
```

`Ctrl+Z` suspends, it does not background by itself.
Use `bg` to continue it in the background.
Use `fg` to resume it in the foreground when needed.

## Keep it running after closing the terminal

If you will close the terminal, detach the job from the shell or run it immune to hangups.

```bash
# Detach the backgrounded job from the shell (bash/zsh built-in)
disown %1

# Or start a new command immune to SIGHUP and keep output
nohup your-command >output.log 2>&1 &

# Alternative: run without controlling terminal
setsid your-command >output.log 2>&1 &
```

`disown` prevents the shell from sending SIGHUP on exit.
`nohup` and `setsid` start the command so it ignores the terminal session entirely.

## Start a new command directly in the background

```bash
your-command &

# Redirect output so it does not clutter your terminal
your-command >out.log 2>&1 &
```

## Tips

- Use job specs like `%1` with `bg`, `fg`, and `disown`.
- Check background activity with `jobs` and process status with `ps`.
- For long-running work on servers, consider `tmux` or `screen` to persist sessions.

