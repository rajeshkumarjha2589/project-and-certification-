# assignment-folder-0
/* assignment solution
Solution in c language */




#include <stdio.h>
#include <math.h>
#include <limits.h>

int get_distance(int x1, int y1, int x2, int y2) {
    int dx = x1 - x2;
    int dy = y1 - y2;
    return (int)sqrt(dx*dx + dy*dy);
}

int get_signal_quality(int q, int d) {
    return q / (1 + d);
}

int is_reachable(int x1, int y1, int x2, int y2, int radius) {
    int d = get_distance(x1, y1, x2, y2);
    return d <= radius ? 1 : 0;
}

int* get_best_coordinate(int towers[][3], int n, int radius) {
    int max_quality = INT_MIN;
    int* min_coordinate = (int*)calloc(2, sizeof(int));
    int i, j, x, y, network_quality, signal_quality, d;

    for (x = -radius; x <= radius; x++) {
        for (y = -radius; y <= radius; y++) {
            network_quality = 0;
            for (i = 0; i < n; i++) {
                if (is_reachable(x, y, towers[i][0], towers[i][1], radius)) {
                    d = get_distance(x, y, towers[i][0], towers[i][1]);
                    signal_quality = get_signal_quality(towers[i][2], d);
                    network_quality += signal_quality;
                }
            }
            if (network_quality > max_quality) {
                max_quality = network_quality;
                min_coordinate[0] = x;
                min_coordinate[1] = y;
            } else if (network_quality == max_quality && (x < min_coordinate[0] || (x == min_coordinate[0] && y < min_coordinate[1]))) {
                min_coordinate[0] = x;
                min_coordinate[1] = y;
            }
        }
    }

    return min_coordinate;
}

int main() {
    int towers[][3] = {{1,2,5},{2,1,7},{3,1,9}};
    int n = sizeof(towers) / sizeof(towers[0]);
    int radius = 2;

    int* best_coordinate = get_best_coordinate(towers, n, radius);
    printf("[%d, %d]\n", best_coordinate[0], best_coordinate[1]);

    return 0;
}



output 
(2,1) as mentioned in assigenment
