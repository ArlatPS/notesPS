# Jest Testing

- describe(name, callback) - for a test suite
- test(name, callback) - test
- expect(function) - prepares value for comparison
- .toBe(value) - shallow comparison
- toStrictlyEqual(array/obj) - deep comparison
- .skip - skips
- beforeEach(callback) - runs before each test in test suite

```
describe("bloom filter", function () {
  let bf;
  beforeEach(() => {
    bf = new BloomFilter();
  });
  test.skip("returns false when empty", () => {
    expect(bf.contains("Brian")).toBe(false);
  });
  test("adding", () => {
    bf.add("Brian");
    expect(bf.contains("Brian")).toBe(true);
    expect(bf.contains("Sarah")).toBe(false);
  });
})
```
