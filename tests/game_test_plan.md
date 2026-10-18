# IT-140 Project Two: Robinson High School Adventure Game
# The player must collect all seven items before entering the cafeteria
# to defeat Demon Joc and save Robinson High School.


def show_instructions():
    """Display the game objective and available commands."""
    print("\nRobinson High School Adventure Game")
    print("Demon Joc has taken over the cafeteria!")
    print("Collect all 7 items before entering the cafeteria to defeat him.")
    print("Movement commands: go north, go south, go east, go west")
    print("Collect items with: get item name")
    print("Type 'quit' if you want to stop playing.")


def show_status(current_room, inventory, rooms):
    """Display the player's location, inventory, room item, and directions."""
    print("\n" + "-" * 40)
    print("You are in:", current_room)
    print("Inventory:", inventory)

    room_item = rooms[current_room].get("item")
    if room_item:
        if room_item not in inventory:
            print("You see:", room_item)
        else:
            print("You have already collected the item in this room.")

    directions = [
        direction for direction in ("north", "south", "east", "west")
        if direction in rooms[current_room]
    ]
    if directions:
        print("Available directions:", ", ".join(directions))
    print("-" * 40)


def main():
    """Run the game until the player wins, loses, or quits."""
    # Each room lists its connected rooms and, when applicable, its item.
    rooms = {
        "Outside Administration": {
            "east": "Administration Office"
        },
        "Administration Office": {
            "west": "Outside Administration",
            "south": "Library",
            "item": "map"
        },
        "Library": {
            "north": "Administration Office",
            "south": "History Room",
            "item": "book"
        },
        "History Room": {
            "north": "Library",
            "east": "Science Room",
            "item": "armor"
        },
        "Science Room": {
            "west": "History Room",
            "south": "English Room",
            "item": "potion"
        },
        "English Room": {
            "north": "Science Room",
            "east": "Shop Room",
            "item": "spell"
        },
        "Shop Room": {
            "west": "English Room",
            "south": "Math Room",
            "item": "sword"
        },
        "Math Room": {
            "north": "Shop Room",
            "west": "Cafeteria",
            "item": "power boost"
        },
        "Cafeteria": {
            "east": "Math Room",
            "villain": "Demon Joc"
        }
    }

    required_items = {
        "map", "book", "armor", "potion", "spell", "sword", "power boost"
    }
    inventory = []
    current_room = "Outside Administration"

    show_instructions()

    while True:
        # Check the villain room immediately upon entering it.
        if current_room == "Cafeteria":
            print("\nYou have entered the Cafeteria.")
            print("Demon Joc is waiting for you!")

            if required_items.issubset(set(inventory)):
                print("Congratulations! You collected all items and defeated Demon Joc!")
                print("You saved Robinson High School!")
            else:
                missing_items = sorted(required_items - set(inventory))
                print("You did not collect all the items.")
                print("Missing items:", ", ".join(missing_items))
                print("NOM NOM... GAME OVER!")
            print("Thanks for playing the game. Hope you enjoyed it.")
            break

        show_status(current_room, inventory, rooms)
        command = input("Enter your move: ").strip().lower()

        if command == "quit":
            print("Thanks for playing Robinson High School Adventure!")
            break

        # Accept movement commands such as "go north".
        if command.startswith("go "):
            direction = command[3:].strip()

            if direction not in ("north", "south", "east", "west"):
                print("Invalid direction. Enter north, south, east, or west.")
            elif direction in rooms[current_room]:
                current_room = rooms[current_room][direction]
                print("You moved", direction + ".")
            else:
                print("You cannot move in that direction from this room.")
                print("Choose one of the available directions.")

        # Accept item commands such as "get map".
        elif command.startswith("get "):
            requested_item = command[4:].strip()
            room_item = rooms[current_room].get("item")

            if room_item is None:
                print("There is no item to collect in this room.")
            elif requested_item != room_item:
                print("That item is not in this room.")
                print("Enter 'get' followed by the item shown in the room.")
            elif room_item in inventory:
                print("You already collected the", room_item + ".")
            else:
                inventory.append(room_item)
                print("You collected the", room_item + "!")
                print("The item has been added to your inventory.")

        else:
            print("Invalid command.")
            print("Use 'go direction', 'get item name', or 'quit'.")


if __name__ == "__main__":
    main()

