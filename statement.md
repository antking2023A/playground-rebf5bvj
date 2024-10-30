#!/usr/bin/perl
use strict;
use warnings;
use List::Util 'shuffle';

# Define the bugs in different zones
my %zones = (
    "Forest" => ["Ant", "Spider", "Beetle", "Centipede", "Firefly", "Dragonfly"],
    "Desert" => ["Scorpion", "Locust", "Lizard", "Beetle", "Sandfly"],
    "Mountain" => ["Eagle", "Mountain Goat", "Moth", "Spider", "Rock Beetle"],
    "Swamp" => ["Mosquito", "Frog", "Snail", "Dragonfly"],
    "City" => ["Cockroach", "Fly", "Ant", "Spider"],
    "Ocean" => ["Jellyfish", "Crab", "Squid", "Sea Turtle"],
    "glichlands" => ["error", "worm", "virus", "backdoor", "ransomware", "the_killer_shn1ckers"],
    "err:_ =name not found =TRUE" => ["err:_ creature_name_not_found", "the_smell_of_the_game.mp4", "error. full_system_death_reset_required", "the_killer_shn1ckers", "stay and you will die.\./", "trojan_horse.exe", "{[code not found]death_file.jpg}"],
    "dear diary" => ["page 1", "random stuff"],
    "murder drones" => ["N", "J", "V", "Tessa", "Cyn", "Uzi", "CynTessa", "Alex", "Thad", "Doll", "Lizzy", "absolutesolver"],
    "FYLYLYLY" => ["FYLY", "SMATATAT", "GICGICLO"],
);

# Initialize the game
my $player_bugs = {};
my $current_zone = "err:_ =name not found =TRUE"; # Starting zone

print "Welcome to battle.org!\n";
print "You are in the $current_zone. What do you want to do?\n";

while (1) {
    print "\nOptions: [1] Fight a Random Bug, [2] Change Zone, [3] View Captured Bugs, [4] Exit
";
    my $choice = <STDIN>;
    chomp($choice);

    if ($choice == 1) {
        # Fight a random bug
        my $bug = get_random_bug($current_zone);
        if ($bug) {
            print "A wild $bug appeared!\n";
            battle($bug);
        } else {
            print "No bugs found in this zone.\n";
        }
    } elsif ($choice == 2) {
        # Change zone
        $current_zone = change_zone();
        print "You've moved to the $current_zone.\n";
    } elsif ($choice == 3) {
        # View captured bugs
        view_captured_bugs();
    } elsif ($choice == 4) {
        print "Thanks for playing!\n";
        last;
    } else {
        print "Invalid choice. Please try again.\n";
    }
}

sub get_random_bug {
    my ($zone) = @_;
    my @bugs = @{ $zones{$zone} || [] };
    return $bugs[rand @bugs] if @bugs;
    return;
}

sub battle {
    my ($bug) = @_;
    my $player_health = 25;
    my $bug_health = int(rand(30)) + 1; # Random health for the bug

    print "You engage in battle with the $bug!\n";
    
    while ($player_health > 0 && $bug_health > 0) {
        print "Your health: $player_health, $bug's health: $bug_health
";
        print "Choose: [1] Attack, [2] Run Away
";
        my $action = <STDIN>;
        chomp($action);
        
        if ($action == 1) {
            my $damage = int(rand(3)) + 1; # Random damage between 1-3
            $bug_health -= $damage;
            print "You dealt $damage damage to $bug!\n";
            
            if ($bug_health > 0) {
                my $bug_damage = int(rand(3)) + 1;
                $player_health -= $bug_damage;
                print "$bug attacked you for $bug_damage damage!\n";
            } else {
                print "You defeated the $bug and captured it!\n";
                $player_bugs->{$bug}++;
            }
        } elsif ($action == 2) {
            print "You ran away from the battle!\n";
            return;
        } else {
            print "Invalid action.\n";
        }
    }

    if ($player_health <= 0) {
        print "You have been defeated!\n";
        exit;
    }
}

sub change_zone {
    my @zone_names = keys %zones;
    return $zone_names[int(rand(@zone_names))]; # Randomly select a zone
}

sub view_captured_bugs {
    print "Captured Bugs:\n";
    for my $bug (keys %$player_bugs) {
        print "$bug: $player_bugs->{$bug}\n";
    }
    print "You haven't captured any bugs yet.\n" unless keys %$player_bugs;
}