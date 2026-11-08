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


## Exercise 2. Find the bug: lost newlines (5 min)
This function is supposed to return the file's exact text. A file containing "a\nb\n" comes back as "ab". Why? Fix it.

std::string readAll(const std::filesystem::path& file) {
    std::ifstream input(file);
    std::string line, text;
    while (std::getline(input, line)) {
        text += line;
    }
    return text;
}

## Corrected code:
std::string readAll(const std::filesystem::path& file) {
    std::ifstream input(file);
    std::ostringstream contents;
    contents << input.rdbuf();
    return contents.str();
}



## Exercise 3. Find the bugs: two path mistakes (5 min)

Both tests below fail on Windows, and the second one fails everywhere. Find both bugs.

```cpp
std::filesystem::path metadataDirectory(const std::filesystem::path& root) {
    return root.string() + "/.minigit";
}
std::filesystem::path headFile(const std::filesystem::path& root) {
    return metadataDirectory(root) / "/HEAD";
}
```

## Corrected code:
std::filesystem::path metadataDirectory(const std::filesystem::path& root) {
    return root / ".minigit";
}

std::filesystem::path headFile(const std::filesystem::path& root) {
    return metadataDirectory(root) / "HEAD";
}


## Exercise 4. Fill in the blanks (5 min)

Complete the missing lines so the function creates missing folders, writes the text, and reports failure.

```cpp
bool save(const std::filesystem::path& file, const std::string& text) {
    if (!file.parent_path().empty()) {                       // (1) only if there is a parent folder
        std::filesystem::create_directories(file.parent_path());   // (2) make all missing folders
    }
    std::ofstream output(file);
    if (!output) {                                       // (3) did opening fail?
        return false;
    }
    output << text;
    return output.good();                              // (4) true only if writing worked
}
```



