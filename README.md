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

#### Pickup / 一部の変更を紹介

##### ・[Block.java](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit/compare/diff?targetBranch=d32e3c764edd6a449ddd220720185d266c2193f9&sourceBranch=refs%2Fheads%2Fmaster&targetRepoId=11#src/main/java/org/bukkit/block/Block.java)

###### Add / 追加

```java
/**
 * Checks if this block is a valid placement location for the specified
 * block data.
 *
 * @param data the block data to check
 * @return <code>true</code> if the block data can be placed here
 */
boolean canPlace(@NotNull BlockData data);
```

`canPlace` というメソッドが追加されました。

ブロックに適用できるブロックデータであるかを判定できます。

##### ・[Player.java](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit/compare/diff?targetBranch=d32e3c764edd6a449ddd220720185d266c2193f9&sourceBranch=refs%2Fheads%2Fmaster&targetRepoId=11#src/main/java/org/bukkit/entity/Player.java)

###### Add / 追加

```java
/**
 * Send the equipment change of an entity. This fakes the equipment change
 * of an entity for a user. This will not actually change the inventory of
 * the specified entity in any way.
 *
 * @param entity The entity that the player will see the change for
 * @param slot The slot of the spoofed equipment change
 * @param item The ItemStack to display for the player
 */
public void sendEquipmentChange(@NotNull LivingEntity entity, @NotNull EquipmentSlot slot, @NotNull ItemStack item);
```

`sendEquipmentChange` というメソッドが追加されました。

エンティティのアーマー変更パケットを送信します。これにより、指定されたエンティティのインベントリが実際に変更されることはありません。

```java
/**
 * Visually hides an entity from this player.
 *
 * @param plugin Plugin that wants to hide the entity
 * @param entity Entity to hide
 * @deprecated draft API
 */
@Deprecated
public void hideEntity(@NotNull Plugin plugin, @NotNull Entity entity);

/**
 * Allows this player to see an entity that was previously hidden. If
 * another another plugin had hidden the entity too, then the entity will
 * remain hidden until the other plugin calls this method too.
 *
 * @param plugin Plugin that wants to show the entity
 * @param entity Entity to show
 * @deprecated draft API
 */
@Deprecated
public void showEntity(@NotNull Plugin plugin, @NotNull Entity entity);

/**
 * Checks to see if an entity has been visually hidden from this player.
 *
 * @param entity Entity to check
 * @return True if the provided entity is not being hidden from this
 *     player
 * @deprecated draft API
 */
@Deprecated
public boolean canSee(@NotNull Entity entity);
```

`hideEntity`, `showEntity`, `canSee` というメソッドが仮追加されました。

プレイヤーからエンティティを見えなくするために使用できます。

```java
/**
 * Shows the demo screen to the player, this screen is normally only seen in
 * the demo version of the game.
 * <br>
 * Servers can modify the text on this screen using a resource pack.
 */
public void showDemoScreen();
```

`showDemoScreen` というメソッドが追加されました。

デモ画面をプレーヤーに表示します。この画面は通常、ゲームのデモバージョンでのみ表示されます。リソースパックを使用してこの画面のテキストを変更できます。

```java
/**
 * Gets whether the player has the "Allow Server Listings" setting enabled.
 *
 * @return whether the player allows server listings
 */
public boolean isAllowingServerListings();
```

`isAllowingServerListings` というメソッドが追加されました。

Minecraft 1.18 では、サーバーリストに自分の名前を表示しないようにするオプションを追加されました。その設定をサーバーから取得することができます。

###### Remove / 削除

```java
/**
 * Send a chunk change. This fakes a chunk change packet for a user at a
 * certain location. The updated cuboid must be entirely within a single
 * chunk. This will not actually change the world in any way.
 * <p>
 * At least one of the dimensions of the cuboid must be even. The size of
 * the data buffer must be 2.5*sx*sy*sz and formatted in accordance with
 * the Packet51 format.
 *
 * @param loc The location of the cuboid
 * @param sx The x size of the cuboid
 * @param sy The y size of the cuboid
 * @param sz The z size of the cuboid
 * @param data The data to be sent
 * @return true if the chunk change packet was sent
 * @deprecated Magic value
 */
@Deprecated
public boolean sendChunkChange(@NotNull Location loc, int sx, int sy, int sz, @NotNull byte[] data);
```

`sendChunkChange` という非推奨メソッドが削除されました。

##### ・[PotionEffectType.java](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit/compare/diff?targetBranch=d32e3c764edd6a449ddd220720185d266c2193f9&sourceBranch=refs%2Fheads%2Fmaster&targetRepoId=11#src/main/java/org/bukkit/potion/PotionEffectType.java)

```diff
- public abstract class PotionEffectType {
+ public abstract class PotionEffectType implements Keyed {
```

`PotionEffectType` に `Keyed` インターフェースが継承されました。

##### [Objective.java](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit/compare/diff?targetBranch=d32e3c764edd6a449ddd220720185d266c2193f9&sourceBranch=refs%2Fheads%2Fmaster&targetRepoId=11#src/main/java/org/bukkit/scoreboard/Objective.java)

```diff
  /**
   * Gets an entry's Score for an Objective on this Scoreboard.
   *
   * @param entry Entry for the Score
   * @return Score tracking the Objective and entry specified
   * @throws IllegalArgumentException if entry is null
   * @throws IllegalStateException if this objective has been unregistered
-  * @throws IllegalArgumentException if entry is longer than 40 characters.
+  * @throws IllegalArgumentException if entry is longer than 32767 characters.
   */
  @NotNull
  Score getScore(@NotNull String entry) throws IllegalArgumentException, IllegalStateException;
```

スコアに使える文字数が 40 から 32767 になりました。

##### [Scoreboard.java](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit/compare/diff?targetBranch=d32e3c764edd6a449ddd220720185d266c2193f9&sourceBranch=refs%2Fheads%2Fmaster&targetRepoId=11#src/main/java/org/bukkit/scoreboard/Scoreboard.java)

```diff
  /**
   * Registers an Objective on this Scoreboard
   *
   * @param name Name of the Objective
   * @param criteria Criteria for the Objective
   * @param displayName Name displayed to players for the Objective.
   * @return The registered Objective
   * @throws IllegalArgumentException if name is null
-  * @throws IllegalArgumentException if name is longer than 16
+  * @throws IllegalArgumentException if name is longer than 32767
   *     characters.
   * @throws IllegalArgumentException if criteria is null
   * @throws IllegalArgumentException if displayName is null
   * @throws IllegalArgumentException if displayName is longer than 128
   *     characters.
   * @throws IllegalArgumentException if an objective by that name already
   *     exists
   */
  @NotNull
  Objective registerNewObjective(@NotNull String name, @NotNull String criteria, @NotNull String displayName) throws IllegalArgumentException;
```

スコアボード名に使える文字数が 40 から 32767 になりました。

### Future API / 将来

> It is leaked that there will be plans underway to change the way  to handle a lot of enums in the API, so that the custom content will be better supported. Need to know,  those changes will not be expected to break most plugin jars, but they may break plugin source code unavoidably.
> 
> If you want to reduce the risk of breakage, make sure to take a consideration to avoid the use of switch statements and EnumSet over enums that implement ‘Keyed’.

カスタムコンテンツがより良くサポートされるように、API で多くの列挙型を扱う方法を変更する計画が進行中であることがリークされています。これらの変更により、ほとんどのプラグインが壊れることはないと思われますが、やむを得ずプラグインのソースコードが壊れる可能性がありますので、ご注意ください。

破損のリスクを減らしたい場合は、`Keyed` を実装した列挙型に対して switch や EnumSet の使用を避けることを考慮してください。

## References / 参考文献

- [Spigot 1.18 Snapshot | AlfinTech Computer](https://www.alfintechcomputer.com/spigot-1-18-snapshot/)
