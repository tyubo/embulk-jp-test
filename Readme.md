# env

- Mac OS High Sierra 10.13.6
- digdag v0.9.33
- java version "1.8.0_161"

# with digdag
```bash
cd with_digdag
docker build . -t jp_test/with_digdag
digdag run jp_test.dig
```

# embulk only
```bash
cd embulk_only
docker build . -t jp_test/embulk_only
docker run -it jp_test/embulk_only bash -c "embulk preview -b /var/lib/embulk jp_test.yml.liquid"
```