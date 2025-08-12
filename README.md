# Install-gf-and-gf-patterns
The `gf` tool, created by Tom Hudson (tomnomnom), is a wrapper around `grep` designed to simplify searching for common patterns in files or data, often used in security research and bug bounty hunting. Below are the steps to install `gf` and its patterns on Kali Linux, assuming you have Go installed (as per your previous query). If Go is not installed, please follow the steps provided earlier to install it first.

### Prerequisites
- **Go installed**: Ensure Go is installed and configured with `$GOPATH` set (e.g., `$HOME/go`).
- **Shell**: Kali Linux uses Zsh by default, but instructions cover both Zsh and Bash.
- **Git**: Ensure `git` is installed (`sudo apt install git`).

### Steps to Install `gf` and `gf` Patterns

1. **Update the System**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install `gf`**:
   Use Go to install the `gf` tool:
   ```bash
   go install github.com/tomnomnom/gf@latest
   ```
   This installs `gf` to `$GOPATH/bin` (e.g., `$HOME/go/bin`). Ensure `$GOPATH/bin` is in your `PATH` (as set up in the Go installation steps: `export PATH=$PATH:$GOPATH/bin`).

3. **Verify `gf` Installation**:
   Check if `gf` is installed correctly:
   ```bash
   gf --help
   ```
   You should see the help output for the `gf` command.

4. **Set Up Autocompletion (Optional)**:
   `gf` provides autocompletion scripts for Bash, Zsh, and Fish. Since Kali uses Zsh by default:
   - For **Zsh**:
     ```bash
     echo 'source $GOPATH/pkg/mod/github.com/tomnomnom/gf@*/gf-completion.zsh' >> ~/.zshrc
     source ~/.zshrc
     ```
     If using Oh-My-Zsh, ensure `gf` is not aliased to `git fetch`:
     ```bash
     unalias gf
     ```
   - For **Bash** (if you switched to Bash):
     ```bash
     echo 'source $GOPATH/pkg/mod/github.com/tomnomnom/gf@*/gf-completion.bash' >> ~/.bashrc
     source ~/.bashrc
     ```

5. **Install `gf` Patterns**:
   `gf` uses JSON files in `~/.gf` to define patterns. You can copy example patterns or use community patterns like those from `1ndianl33t/Gf-Patterns`.
   - **Option 1: Copy Example Patterns from `gf`**:
     ```bash
     mkdir ~/.gf
     cp -r $GOPATH/pkg/mod/github.com/tomnomnom/gf@*/examples/* ~/.gf/
     ```
     This copies the example patterns (e.g., `php-sources.json`, `urls.json`) to `~/.gf`.
   - **Option 2: Install `1ndianl33t/Gf-Patterns` (Community Patterns)**:
     Clone the repository for additional patterns (e.g., SSRF, RCE, SQLi):
     ```bash
     git clone https://github.com/1ndianl33t/Gf-Patterns ~/Gf-Patterns
     mkdir ~/.gf
     mv ~/Gf-Patterns/*.json ~/.gf/
     ```
     These patterns include `ssrf.json`, `rce.json`, `sqli.json`, etc., tailored for security testing.[](https://github.com/1ndianl33t/Gf-Patterns)

6. **Verify Patterns**:
   List available patterns:
   ```bash
   gf -list
   ```
   You should see patterns like `ssrf`, `redirect`, `php-sources`, etc., depending on the patterns installed.

7. **Test `gf` with Patterns**:
   Test `gf` with a pattern (e.g., `ssrf`):
   ```bash
   echo "https://example.com?redirect=https://evil.com" | gf ssrf
   ```
   If configured correctly, `gf` will match lines containing SSRF-related parameters (e.g., `redirect=`). You can also use it with files:
   ```bash
   cat urls.txt | gf ssrf
   ```

8. **Optional: Install `waybackurls` for Enhanced Usage**:
   `gf` is often used with tools like `waybackurls` for web crawling. Install it:
   ```bash
   go install github.com/tomnomnom/waybackurls@latest
   ```
   Example usage:
   ```bash
   cat subdomains.txt | waybackurls | sort -u | gf ssrf | tee -a ssrfparams.txt
   ```
   This fetches URLs from subdomains and filters for SSRF patterns.[](https://github.com/1ndianl33t/Gf-Patterns)

### Troubleshooting
- **"command not found: gf"**: Ensure `$GOPATH/bin` is in your `PATH`. Run `export PATH=$PATH:$GOPATH/bin` and verify with `echo $PATH`.
- **No patterns found**: Check if `~/.gf` exists and contains `.json` files. If not, re-copy the pattern files.
- **Zsh alias conflict**: If `gf` runs `git fetch`, run `unalias gf` in your terminal or add it to `~/.zshrc`.
- **Go module issues**: If `go install` fails, ensure your Go version is up-to-date (`go version`) and try clearing the Go cache:
  ```bash
  go clean -modcache
  ```

### Notes
- The `gf` tool requires pattern files in `~/.gf` to function effectively. Community patterns like `1ndianl33t/Gf-Patterns` are popular for security researchers.[](https://github.com/1ndianl33t/Gf-Patterns)
- Regularly update `gf` and patterns:
  ```bash
  go install github.com/tomnomnom/gf@latest
  git -C ~/Gf-Patterns pull
  mv ~/Gf-Patterns/*.json ~/.gf/
  ```
- For more patterns, explore repositories like `B1gN0Se/gf-patterns`, which offer automated installation scripts.[](https://github.com/B1gN0Se/gf-patterns/blob/main/README.md)[](https://github.com/B1gN0Se/gf-patterns)
- For video tutorials, search YouTube for “install gf tool Kali Linux” (e.g., channels like BugsX404 or CodeGrills).[](https://www.youtube.com/watch?v=OTiEWHM-AyM)[](https://www.youtube.com/watch?v=dKyA8iWvn28)

For further details, refer to the official `gf` GitHub page (https://github.com/tomnomnom/gf) or the `Gf-Patterns` repository (https://github.com/1ndianl33t/Gf-Patterns).[](https://github.com/tomnomnom/gf)[](https://github.com/1ndianl33t/Gf-Patterns)
