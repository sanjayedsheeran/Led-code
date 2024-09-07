#include <stdio.h>
#include <stdint.h>

// Structure to represent individual LED settings
typedef struct {
    uint8_t state;        // 1 for ON, 0 for OFF
    uint8_t brightness;   // Brightness level (0–255)
    uint32_t color;       // RGB color encoded in a single unsigned integer
} LEDSettings;

// Structure to represent the group of LEDs
typedef struct {
    LEDSettings singleLED;   // Individual LED settings
    uint8_t groupState;      // Collective state of LEDs (1 for ON, 0 for OFF)
    uint8_t groupBrightness; // Collective brightness (0–255)
} LEDGroup;

// Function to initialize LEDGroup with default values
void initLEDGroup(LEDGroup *group) {
    group->singleLED.state = 0;          // LED is OFF
    group->singleLED.brightness = 0;     // Minimum brightness
    group->singleLED.color = 0x000000;   // Black (no color)
    group->groupState = 0;               // Group LEDs are OFF
    group->groupBrightness = 0;          // Group brightness is minimum
}

// Function to update individual and group LED settings
void updateLEDGroupSettings(LEDGroup *group, uint8_t groupState, uint8_t groupBrightness, uint8_t state, uint8_t brightness, uint32_t color) {
    group->groupState = groupState;
    group->groupBrightness = groupBrightness;
    
    // Update individual LED settings
    group->singleLED.state = state;
    group->singleLED.brightness = brightness;
    group->singleLED.color = color;
}

// Function to display the current LED group status
void displayLEDGroupStatus(const LEDGroup *group) {
    printf("Individual LED State: %s\n", group->singleLED.state ? "ON" : "OFF");
    printf("Individual LED Brightness: %u\n", group->singleLED.brightness);
    printf("Individual LED Color (RGB): #%06X\n", group->singleLED.color);
    printf("Group State: %s\n", group->groupState ? "ON" : "OFF");
    printf("Group Brightness: %u\n", group->groupBrightness);
}

int main() {
    // Create an LEDGroup instance
    LEDGroup group;

    // Initialize the group with default values
    initLEDGroup(&group);

    // Display the initial status
    displayLEDGroupStatus(&group);

    // Update group and individual LED settings
    updateLEDGroupSettings(&group, 1, 128, 1, 255, 0xFF0000);  // Group ON, Group Brightness 128, LED ON, Brightness 255, Color Red

    // Display the updated status
    displayLEDGroupStatus(&group);

    return 0;
}
