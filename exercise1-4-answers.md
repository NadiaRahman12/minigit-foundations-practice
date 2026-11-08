## Exercise 1. Predict the output (5 min)

Write down what each line prints, then check with a small program. Assume Linux.

```cpp
using std::filesystem::path;                                            // Expected output
std::cout << (path("repo") / ".minigit" / "HEAD").generic_string();     // repo/.minigit/HEAD
std::cout << (path("repo") / "/HEAD").generic_string();                 // /HEAD
std::cout << path("docs/notes.txt").filename().string();                // notes.txt
std::cout << path("docs/notes.txt").parent_path().string();             // docs
std::cout << path("notes.txt").parent_path().empty();                   // 1
std::cout << path("notes.txt").extension().string();                    // .txt
```
