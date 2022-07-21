## How to pull and compile this:
Similar to the tutorial at https://chromium.googlesource.com/chromium/src/+/main/docs/linux/build_instructions.md
  - Install `depot_tools` (`git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git && export PATH="$PATH:${HOME}/depot_tools"`
  - Make a `chromium` directory: `mkdir ~/chromium && cd ~/chromium`
  - Run the following command:
    ```
    gclient config --spec='solutions=[
      {
        "name":"src", 
        "url":"https://github.com/justus237/chromium.git", 
        "custom_deps":{},
        "custom_vars": {},
      },
    ]'
    ```
    - This repository has a modified DEP file that points to our own fork of quiche without a specific commit hash, so that the latest version (`HEAD`) is always fetched. Note that this quiche fork still uses the old directory structure where everything is in `net/third_party/quiche/src` and not in `net/third_party/quiche/src/quiche`
  - run ```gclient sync --nohooks``` (no history will fail because nobody is interested in fixing shallow clones for one of the third party repositories, see https://groups.google.com/a/chromium.org/g/chromium-dev/c/pRDAs6tm-Zs, https://bugs.chromium.org/p/chromium/issues/detail?id=1313310, https://bugs.chromium.org/p/chromium/issues/detail?id=1226496)
  - ```cd src```
  - ```./build/install-build-deps.sh --no-nacl```
    - if there are dependency issues, looking at https://github.com/ValveSoftware/steam-for-linux/issues/7982 might help
  - ```gn gen out/Default```
  - ```gn args out/Default```
    - values to set:
      - ```symbol_level = 0```
      - ```is_debug=false```
      - ```enable_nacl=false```
      - ```dcheck_always_on=false```
  - ```autoninja -C out/Default chrome chromedriver```

### Changes in this repository
- `build/write_build_date_header.py` timestamps with the current date because it was breaking on the machines provided by the chair
- DEPS is updated to point to `https://github.com/justus237/quiche`

# ![Logo](chrome/app/theme/chromium/product_logo_64.png) Chromium

Chromium is an open-source browser project that aims to build a safer, faster,
and more stable way for all users to experience the web.

The project's web site is https://www.chromium.org.

To check out the source code locally, don't use `git clone`! Instead,
follow [the instructions on how to get the code](docs/get_the_code.md).

Documentation in the source is rooted in [docs/README.md](docs/README.md).

Learn how to [Get Around the Chromium Source Code Directory Structure
](https://www.chromium.org/developers/how-tos/getting-around-the-chrome-source-code).

For historical reasons, there are some small top level directories. Now the
guidance is that new top level directories are for product (e.g. Chrome,
Android WebView, Ash). Even if these products have multiple executables, the
code should be in subdirectories of the product.

If you found a bug, please file it at https://crbug.com/new.
