import pandas as pd
import numpy as np
from sklearn.utils import resample

# Assuming you have a DataFrame 'original_data' with your existing data
# For demonstration purposes, I'll create a simplified example DataFrame
data = {
    'Date (week ending)': ['30/07/2023', '30/07/2023', '30/07/2023', '30/07/2023', '30/07/2023', '30/07/2023', '23/07/2023', '23/07/2023', '23/07/2023', '23/07/2023'],
    'Location': ['Bama', 'Dikwa', 'Mafa', 'Katagum', 'Maduganari', 'Yola North', 'Monguno', 'Mobbar', 'Gwoza', 'Dikwa'],
    'States': ['Borno', 'Borno', 'Borno', 'Bauchi', 'Borno', 'Adamawa', 'Borno', 'Borno', 'Borno', 'Borno'],
    'Category': ['Armed conflict', 'Armed conflict', 'Armed conflict', 'Civil unrest', 'Criminality', 'Criminality', 'Armed conflict', 'Military operations', 'Terrorism', 'Farmers/IDP relocation'],
    'OSS Highlights': [
        'Armed attacks were reported in Bama, Dikwa, and Mafa, LGA, Borno State.',
        'Armed attacks were reported in Bama, Dikwa, and Mafa, LGA, Borno State.',
        'Armed attacks were reported in Bama, Dikwa, and Mafa, LGA, Borno State.',
        '',  # Replace empty string randomly
        '',
        '47 arrested in Adamawa as youth carrying machetes and baton sticks vandalize Adamawa state government house, airport, warehouses and shops in Yola North and South.',
        'MOPOL stabbed to death at the Artillery Mammy Market in Maiduguri by a suspected drug dealer.',
        '',  # Replace empty string randomly
        '',
        ''
    ],
    'Casualty': np.random.randint(0, 21, size=10),
    'Injuries': np.random.randint(0, 11, size=10),
    'Fatality': np.random.randint(0, 16, size=10),
    'Other': np.random.randint(0, 48, size=10)
}

original_data = pd.DataFrame(data)

# Replace empty strings in 'OSS Highlights' randomly with existing non-empty values
non_empty_highlights = original_data.loc[original_data['OSS Highlights'] != '', 'OSS Highlights']
empty_indices = original_data[original_data['OSS Highlights'] == ''].index

original_data.loc[empty_indices, 'OSS Highlights'] = np.random.choice(non_empty_highlights, len(empty_indices))

# Data augmentation using bootstrapping
augmented_data = pd.concat([resample(original_data) for _ in range(5000 // len(original_data))])

# Optionally, you can reset the index of the augmented DataFrame
augmented_data.reset_index(drop=True, inplace=True)

# Now 'augmented_data' contains your simulated dataset with 5000 rows

# Check the first few rows of the augmented dataset
print(augmented_data.head())
