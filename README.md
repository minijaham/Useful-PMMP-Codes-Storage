# Useful-PMMP-Codes-Storage
A public short code storage for myself...

# Double S Symbol
§

# Editing Base Damage
```php
$event->setBaseDamage($event->getBaseDamage() * $multiplier);
```
# Get Item In Hand's name
```php
$player->getInventory()->getItemInHand()->getName() === "item name"
```
# Register Command
```php
$this->getServer()->getCommandMap()->register("cmd", new CmdCommand("cmd", $this));
```
# Register Event
```php
$this->getServer()->getPluginManager()->registerEvents(new EventListener($this), $this);
```
# onCommand Function
```php
public function onCommand(CommandSender $player, Command $command, string $label, array $args): bool
    {
        if ($command->getName() === "cmd") {
        }
        return true;
    }
```
# Lightning Entity
```php
<?php

declare(strict_types=1);

namespace minijaham\MMORPG\Entity;

use pocketmine\block\Block;
use pocketmine\entity\Entity;
use pocketmine\entity\EntityIds;
use pocketmine\entity\Living;
use pocketmine\event\entity\EntityCombustByEntityEvent;
use pocketmine\event\entity\EntityDamageByEntityEvent;
use pocketmine\event\entity\EntityDamageEvent;
use pocketmine\Player;

class Lightning extends Entity
{
    const NETWORK_ID = EntityIds::LIGHTNING_BOLT;
    /** @var float */
    public $width = 0.3;
    /** @var float */
    public $length = 0.9;
    /** @var float */
    public $height = 1.8;
    /** @var int */
    protected $age = 0;

    public function entityBaseTick(int $tickDiff = 1): bool
    {
        if ($this->closed) {
            return false;
        }
        $this->age += $tickDiff;
        $hasUpdate = parent::entityBaseTick($tickDiff);
        foreach ($this->getLevel()->getNearbyEntities($this->getBoundingBox()->expandedCopy(4, 3, 4), $this) as $entity) {
            if ($entity instanceof Living && $entity->isAlive()) {
                $owner = $this->getOwningEntity();
                if (!$owner instanceof Player) {
                    $ev = new EntityCombustByEntityEvent($this, $entity, mt_rand(3, 8));
                    $ev->call();
                    $entity->setOnFire($ev->getDuration());
                }
                $ev = new EntityDamageByEntityEvent($this, $entity, EntityDamageEvent::CAUSE_CUSTOM, 5);
                $ev->call();
                $entity->attack($ev);
            }
        }
        if ($this->age > 20) {
            $this->flagForDespawn();
        }
        return $hasUpdate;
    }
}
```

# Task Scheduling
```php
<?php

namespace minijaham\MMORPG\Tasks;

use pocketmine\scheduler\Task;

class TaskClass extends Task
{
    private $plugin;

    public function __construct(Main $plugin)
    {
        $this->plugin = $plugin;
    }

    public function onRun(int $tick)
    {
        // Code
    }
}
```
# Getting player direction
```php
if ($player->getDirection() === 2) {
    echo "North";
}
if ($player->getDirection() === 3) {
    echo "East";
}
if ($player->getDirection() === 0) {
    echo "South";
}
if ($player->getDirection() === 1) {
    echo "West";
}
```
