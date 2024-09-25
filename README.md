# POO_Reto_8

"Be creative". I tried as much as I could. 

The purpose of this code is to model how plants with different nutrient requirements can be fertilized effectively using an object-oriented programming approach. The _Plant_ class serves as the foundation, representing any type of plant, and its subclasses are categorized based on their nutrient needs, such as heavy feeders, nitrogen-fixing plants, or fruiting crops. Similarly, the _Fertilizer_ class models various types of fertilizers, like nitrogen-rich, phosphorus-rich, and potassium-rich fertilizers, with their subclasses providing specific fertilizer types that correspond to the nutrient needs of different plants.

In this model, plants "request" specific fertilizers based on their characteristics, and fertilizers "recommend" themselves based on the type of plant they are suitable for. This allows for a highly customizable and efficient system where each plant gets exactly the nutrients it needs for optimal growth, whether it be nitrogen for leafy vegetables, phosphorus for root crops, or potassium for fruiting plants.

## How can this be used in the real world?

This code can be integrated into a smart farming system to automate fertilization processes. For example, using sensors and IoT devices, real-time plant health data could be fed into this model, and the system would determine which fertilizers to apply based on the type of crop being grown, saving time and resources.

The model could be used in agricultural software to help farmers plan and optimize fertilization schedules, ensuring that each crop gets the nutrients it needs at the right time, thus maximizing yields and reducing waste.

It could also serve as a learning tool in agricultural science programs to teach students how different plants respond to various nutrients, and how to design fertilization plans based on real-world crop management scenarios.

## The code

Being on the early developement stage, this code is meant to iterate over the classes Plant and Fertilizer, and return recommendations based on their suitability.

```
# Superclass for Plant
class Plant:
    def __init__(self, name, water_needs, growth_rate, nutrient_needs):
        self.name = name
        self.water_needs = water_needs
        self.growth_rate = growth_rate
        self.nutrient_needs = nutrient_needs

    def recommend_fertilizer(self):
        raise NotImplementedError("This method should be implemented by subclasses")

    def __lt__(self, other):
        # Custom sorting logic based on growth_rate and nutrient_needs
        growth_rate_order = {"Slow": 0, "Medium": 1, "Fast": 2}
        nutrient_needs_order = {"Low": 0, "Medium": 1, "High": 2}

        if growth_rate_order[self.growth_rate] != growth_rate_order[other.growth_rate]:
            return growth_rate_order[self.growth_rate] < growth_rate_order[other.growth_rate]
        return nutrient_needs_order[self.nutrient_needs] < nutrient_needs_order[other.nutrient_needs]

class PlantIterator:
    def __init__(self, plants):
        self.plants = sorted(plants)
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index < len(self.plants):
            plant = self.plants[self.index]
            self.index += 1
            return plant
        else:
            raise StopIteration

# Subclass for heavy feeder plants
class HeavyFeederPlant(Plant):
    def __init__(self, name, water_needs, growth_rate):
        super().__init__(name, water_needs, growth_rate, "High")

    def recommend_fertilizer(self):
        fertilizers = []
        if self.growth_rate == "Fast":
            fertilizers.append("NitrogenRichFertilizer")
        if self.nutrient_needs == "High":
            fertilizers.append("BalancedFertilizer")
        return fertilizers

# Subclass for nitrogen-fixing plants
class NitrogenFixingPlant(Plant):
    def __init__(self, name, water_needs, growth_rate):
        super().__init__(name, water_needs, growth_rate, "Medium")

    def recommend_fertilizer(self):
        fertilizers = []
        if self.growth_rate == "Medium":
            fertilizers.append("PhosphorusRichFertilizer")
        return fertilizers

# Subclass for fruiting crops
class FruitingCrop(Plant):
    def __init__(self, name, water_needs, growth_rate):
        super().__init__(name, water_needs, growth_rate, "Medium")

    def recommend_fertilizer(self):
        fertilizers = []
        if self.growth_rate == "Fast":
            fertilizers.append("PotassiumRichFertilizer")
        return fertilizers

# Superclass for Fertilizer
class Fertilizer:
    def __init__(self, name, composition, frequency):
        self.name = name
        self.composition = composition
        self.frequency = frequency

    def recommend_plants(self):
        raise NotImplementedError("This method should be implemented by subclasses")

    def __lt__(self, other):
        # Custom sorting logic based on composition and frequency
        composition_order = {"N-rich": 0, "P-rich": 1, "K-rich": 2}
        frequency_order = {"Every 2 weeks": 0, "Monthly": 1}

        if composition_order[self.composition] != composition_order[other.composition]:
            return composition_order[self.composition] < composition_order[other.composition]
        return frequency_order[self.frequency] < frequency_order[other.frequency]

class FertilizerIterator:
    def __init__(self, fertilizers):
        self.fertilizers = sorted(fertilizers)
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index < len(self.fertilizers):
            fertilizer = self.fertilizers[self.index]
            self.index += 1
            return fertilizer
        else:
            raise StopIteration

# Subclass for Nitrogen-rich Fertilizer
class NitrogenRichFertilizer(Fertilizer):
    def recommend_plants(self):
        plants = []
        if self.composition == "N-rich" and self.frequency == "Every 2 weeks":
            plants.append("HeavyFeederPlant")
            plants.append("LeafyVegetable")
        return plants

# Subclass for Phosphorus-rich Fertilizer
class PhosphorusRichFertilizer(Fertilizer):
    def recommend_plants(self):
        plants = []
        if self.composition == "P-rich" and self.frequency == "Monthly":
            plants.append("NitrogenFixingPlant")
            plants.append("RootCrop")
        return plants

# Subclass for Potassium-rich Fertilizer
class PotassiumRichFertilizer(Fertilizer):
    def recommend_plants(self):
        plants = []
        if self.composition == "K-rich" and self.frequency == "Every 2 weeks":
            plants.append("FruitingCrop")
        return plants

# Main
tomato = FruitingCrop("Tomato", "Medium", "Fast")
bean = NitrogenFixingPlant("Bean", "Low", "Medium")
corn = HeavyFeederPlant("Corn", "High", "Fast")

plants = [tomato, bean, corn]
plant_iterator = PlantIterator(plants)

for plant in plant_iterator:
    print(f"{plant.name}: {plant.growth_rate}, {plant.nutrient_needs}")

nitrogen_fertilizer = NitrogenRichFertilizer("Urea", "N-rich", "Every 2 weeks")
phosphorus_fertilizer = PhosphorusRichFertilizer("Bone Meal", "P-rich", "Monthly")
potassium_fertilizer = PotassiumRichFertilizer("Potash", "K-rich", "Every 2 weeks")

fertilizers = [nitrogen_fertilizer, phosphorus_fertilizer, potassium_fertilizer]
fertilizer_iterator = FertilizerIterator(fertilizers)

for fertilizer in fertilizer_iterator:
    print(f"{fertilizer.name}: {fertilizer.composition}, {fertilizer.frequency}")

print(tomato.recommend_fertilizer())
print(bean.recommend_fertilizer())
print(corn.recommend_fertilizer())

print(nitrogen_fertilizer.recommend_plants())
print(phosphorus_fertilizer.recommend_plants())
print(potassium_fertilizer.recommend_plants())
```
