# Spigot Changes 1.18

## Changes / 変更点

### Java Version / Java バージョン

> Since Mojang has decided to create Minecraft 1.18, sure it will require Java 17 or later. To get the version, you may be able to install it from a third party, such as Azul Zulu or your Linux package manager.
>
> It’s important to note, the –illegal-access=permit workaround is no longer possible on this version. Keep in mind, the current version of Java 17 actually has a bug that may affect users on single-core systems. If you already have such a system, take a consideration to add ‘Djava.util.concurrent.ForkJoinPool.common.parallelism=1’ as a Java argument.

Minecraft 1.18 からは Java17 以降が必要になりました。Azul Zulu や Linux パッケージマネージャーなどのサードパーティツールからインストールできます。

このバージョンでは、`–illegal-access=permit` が使用できなくなりました。また、現在の Java17 には、シングルコアシステムのユーザーに影響を与える可能性のあるバグがあります。すでにそのようなシステムを使用している場合は、Java 引数として `Djava.util.concurrent.ForkJoinPool.common.parallelism=1` を追加することを検討してください。

### Backups / バックアップ

> Before upgrading, ensure that you have recent and tested backups for your server. As usual, it may not be possible to downgrade your server to an earlier version. However, it is an important thing for this release, because of the permanent alterations to older worlds.

アップグレードする前に、サーバーのバックアップを必ず行ってください。古いワールドデータに対して、恒久的な変更が加われるため、ダウングレードすることができない場合があります。

### API Changes / API の変更

> You may also need to view a complete summary of the API changes between 1.17.1 and 1.18.x through this [link](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit/compare/diff?targetBranch=d32e3c764edd6a449ddd220720185d266c2193f9&sourceBranch=refs%2Fheads%2Fmaster&targetRepoId=11). Need to know, there are no intentional API breakages, however there may be slight unavoidable changes. So, here’s a list of API changes that includes:
> - Java 17 comes to change behaviour, especially if reflection is used.
> - MySQL has been upgraded to the 8.x driver series. Furthermore, this driver may be more strict in some operations.
> - Google GSON and Guava have been bumped to the newer versions per Mojang.
> - A number of biomes actually have been renamed or deleted.
> - The world height will extend to less than 0 and greater than 256.
> - There might have been extensive internal changes to biome code and world generation. You can also report as bugs about any plugin-facing changes at this stage are not intentional.

また、1.17.1 と 1.18.x 間のAPI変更点は、こちらの[リンク](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit/compare/diff?targetBranch=d32e3c764edd6a449ddd220720185d266c2193f9&sourceBranch=refs%2Fheads%2Fmaster&targetRepoId=11) から確認できます。意図的なAPIの破損はありませんが、やむを得ない若干の変更があることを知っておく必要があります。そこで、以下にAPIの変更点を列挙します。

- リフレクションを使用している場合、Java17 で動作が変更されます。
- MySQL が 8.x ドライバシリーズにアップグレードされました。さらに、このドライバーは、いくつかの操作でより厳密になる可能性があります。
- Google GSON と Guava は Mojang によって新しいバージョンにアップグレードされました。
- いくつかのバイオーム名が変更・削除されました。
- ワールドの高さが 0~256 より大きくなりました。
- バイオームのコードとワールドの生成に大規模な内部変更があったかもしれません。また、現段階でのプラグイン向けの変更は意図的なものではないことについて、バグとして報告することができます。

### Future API / 将来

> It is leaked that there will be plans underway to change the way  to handle a lot of enums in the API, so that the custom content will be better supported. Need to know,  those changes will not be expected to break most plugin jars, but they may break plugin source code unavoidably.
> 
> If you want to reduce the risk of breakage, make sure to take a consideration to avoid the use of switch statements and EnumSet over enums that implement ‘Keyed’.

カスタムコンテンツがより良くサポートされるように、API で多くの列挙型を扱う方法を変更する計画が進行中であることがリークされています。これらの変更により、ほとんどのプラグインが壊れることはないと思われますが、やむを得ずプラグインのソースコードが壊れる可能性がありますので、ご注意ください。

破損のリスクを減らしたい場合は、`Keyed` を実装した列挙型に対して switch や EnumSet の使用を避けることを考慮してください。

## References / 参考文献

- [Spigot 1.18 Snapshot | AlfinTech Computer](https://www.alfintechcomputer.com/spigot-1-18-snapshot/)
