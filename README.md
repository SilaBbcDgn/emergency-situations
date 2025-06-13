import random
import time

# Train and track data
trains = [1,2,3,4,5,6]
tracks = ["A", "B", "C", "D", "E"]
signal_states = ["Red", "Yellow", "Green"]
emergency_types = ["Signal Failure", "Train Delay", "Train Breakdown", "Track Obstruction"]

# Dictionaries to store train and track data
train_routes = {}
track_signals = {track: random.choice(signal_states) for track in tracks}

# Function to assign random routes to trains
def assign_train_routes():
    """
    Assigns random routes to trains, ensuring no route is repeated.
    """
    available_tracks = tracks.copy()
    for train in trains:
        if not available_tracks:
            print("Error: Not enough tracks for all trains!")
            return False
        
        selected_track = random.choice(available_tracks)
        available_tracks.remove(selected_track)
        train_routes[train] = selected_track
    print("Train Routes Assigned:", train_routes)

# Function to update signal states
def update_signals():
    """
    Updates the signal state for all tracks randomly.
    """
    for track in track_signals:
        current_signal = track_signals[track]
        possible_states = [state for state in signal_states if state != current_signal]
        track_signals[track] = random.choice(possible_states)

# Function to simulate emergencies
def simulate_emergency():
    """
    Randomly selects an emergency type and applies it to a train or track.
    """
    emergency = random.choice(emergency_types)
    if emergency == "Signal Failure":
        affected_track = random.choice(tracks)
        print(f"Emergency: Signal Failure on track {affected_track}!")
        track_signals[affected_track] = "Red"
    elif emergency == "Train Delay":
        if train_routes:
            affected_train = random.choice(list(train_routes.keys()))
            print(f"Emergency: Train {affected_train} is delayed on track {train_routes[affected_train]}!")
        else:
            print("No trains assigned to routes. Skipping 'Train Delay' emergency.")
    elif emergency == "Train Breakdown":
        if train_routes:
            affected_train = random.choice(list(train_routes.keys()))
            print(f"Emergency: Train {affected_train} has broken down on track {train_routes[affected_train]}!")
            del train_routes[affected_train]
            print(f"Train {affected_train} removed from operation.")
        else:
            print("No trains assigned to routes. Skipping 'Train Breakdown' emergency.")
    elif emergency == "Track Obstruction":
        affected_track = random.choice(tracks)
        print(f"Emergency: Obstruction detected on track {affected_track}!")
        for train, route in train_routes.items():
            if route == affected_track:
                print(f"Train {train} stopped due to obstruction on track {affected_track}.")

# Function to simulate the system over time
def run_simulation(duration=20, interval=3):
    """
    Simulates the system with updates to signals and random emergencies.
    :param duration: Total time to run the simulation (seconds).
    :param interval: Time interval between updates (seconds).
    """
    start_time = time.time()
    while time.time() - start_time < duration:
        # Print current signal states
        print("\nCurrent Signal States:")
        for track, signal in track_signals.items():
            print(f"  Track {track}: {signal}")
        
        # Trigger a random emergency
        simulate_emergency()
        
        # Update signal states
        update_signals()
        
        print("-" * 40)
        time.sleep(interval)

# Main program
assign_train_routes()  # Assign routes to trains
run_simulation()       # Start the simulation
