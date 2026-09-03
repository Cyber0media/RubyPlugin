# RubyCraft Plugin (Paper/Spigot 1.20.4)

## Derleme
```
mvn clean package
```
Çıktı: `target/RubyCraft.jar` → sunucunun `plugins/` klasörüne kopyalayın.

## Mimari özeti
- `items/RubyItems.java` — Tüm Yakut eşyalarının tek kaynak noktası.
  Her eşya, mevcut bir vanilla materyali (Netherite alet/zırh, Amethyst
  Shard, Raw Copper Block) taban alıp `CustomModelData` + PDC etiketi +
  `Component.translatable(...)` ile "Yakut"laştırılır.
- `recipes/RecipeRegistrar.java` — Tüm crafting tariflerini (blok
  dönüşümü, aletler, zırhlar) `onEnable()` içinde kaydeder.
- `listeners/OreBreakListener.java` — Yakut Cevheri kırılınca vanilla
  düşümünü iptal eder, Elmas seviyesinde XP (3-7) verir, Fortune
  büyüsüne göre rastgele Yakut miktarı düşürür.
- `listeners/AnvilRepairListener.java` — Yakut eşyalarının örste
  Netherite Ingot yerine ham Yakut ile tamir edilmesini sağlar.
  **Büyü uyumluluğu için bu sınıfa ihtiyaç YOKTUR** — Netherite tabanlı
  olduğu için tüm Elmas/Netherite büyüleri zaten native çalışır.

## Neden ekstra bir "enchant izin" listener'ı yok?
Bukkit'te bir büyünün bir eşyaya uygulanabilirliği
`Enchantment#canEnchantItem(ItemStack)` ile belirlenir ve bu, eşyanın
gerçek `Material` türüne bakar. Yakut eşyaları gerçekte hâlâ
`NETHERITE_SWORD`, `NETHERITE_PICKAXE` vb. olduğu için büyü masası ve
örs bunları otomatik olarak Netherite/Elmas eşyası gibi kabul eder —
davranışı değiştirmeye çalışmak gereksiz ve riskli olurdu.
