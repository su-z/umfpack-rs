This repo modifies the original [https://github.com/flaport/umfpack-rs]() repo by changing its `build.rs` script to make. The changes are as follows.

```rust
--- a/build.rs
+++ b/build.rs
@@ -4,9 +4,17 @@ use std::fs::read_dir;
 use std::path::Path;
 
 fn main() {
+    if env::var("TARGET").unwrap().contains("apple-darwin") {
+        println!("cargo:warning=Building for macOS");
+        env::set_var("CC", "gcc");
+        println!("cargo:rustc-link-search=native=/usr/local/lib");
+        println!("cargo:rustc-link-lib=dylib=omp");
+    }
+    else {
+        println!("cargo:rustc-link-search=native=/usr/lib");
+        println!("cargo:rustc-link-lib=dylib=gomp");
+    }
     println!("cargo:include=/usr/include");
-    println!("cargo:rustc-link-search=native=/usr/lib");
-    println!("cargo:rustc-link-lib=dylib=gomp");
     println!("cargo:rustc-link-lib=dylib=blas");
 
     let ss_dir = clone_suitesparse();
```
