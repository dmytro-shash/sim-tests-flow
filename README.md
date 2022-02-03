# sim-tests-flow

общая документация для ознакомления, лучше сначала прочитать для чего нужны и что это такое


https://www.near-sdk.io/testing/simulation-tests

https://crates.io/crates/near-sdk-sim

## клонируем пример для теста из near-examples/ft

> git clone https://github.com/near-examples/FT.git

там уже указаны все депендеси, но для проекта нужно указать зависимости в cargo.toml [тот что хочешь тестировать, например dtoken/Cargo.toml]

in nearland-protocol/contracts/dtoken/Cargo.toml
```
[dev-dependencies]
near-sdk-sim = "3.2.0"
```

## в той же папке dtoken 
создаешь папку tests и в ней еще папку sim, получается dtoken/tests/sim 
(смотр. на пример из ft - единственный контракт)


включаем в общий файл зависимости Cargo.toml папки для тестирования

in nearland-protocol/Cargo.toml

```
[workspace]
# remember to include a member for each contract
members = [
    "contracts/controller",
    "contracts/dtoken",
    "contracts/interest_rate_model",
    "contracts/token_mock"
]
```

дальше для тестирование сначала нужно сбилдить проект и "указать тестам какой исходный wasm файл использовать для тестирования"

или скрипт ./build.sh и смотреть где лежит исходный wasm file
или cargo build

создаем внутри nearland-protocol/contracts/dtoken/tests/sim файл utils.rs  и подключаем васм файл, который сбилдили

```
near_sdk_sim::lazy_static_include::lazy_static_include_bytes! {
TOKEN_WASM => "../../res/dtoken.wasm",  ---------------- тут может быть твой путь к wasm файлу
}
```

дальше по примеру можно написать тесты, смотри near-examles/FT, запускаются обичной командой
> cargo test



## проблемы которые могут возникнуть во время билда это не установленные пакети типа 

![img.png](img.png)

резольвится с помощью установки библиотек, см. https://programmerah.com/solved-error-failed-to-run-custom-build-command-for-librocksdb-sys-v6-17-3-32728/






